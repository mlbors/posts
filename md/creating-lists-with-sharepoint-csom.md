# Creating lists with SharePoint CSOM #

In this post, we are going to see how we can work with _SharePoint_ lists using _CSOM_.

## Introduction ##

Since _SharePoint_ 2010, _SharePoint_ provides us a way to interact with _SharePoint_ sites called _Client Object Model_, or _CSOM_. So we are able to write scripts or _Add-Ins_ without the need to program directly on the server where _SharePoint_ is installed.

Here, we are going to create a small program to see how we can use _SharePoint_ _CSOM_ to work with lists. For the sake of our example, we state that we are using a _SharePoint_ _on-premise_ installation.

## Time to code ##

First, let's create a method to connect to _SharePoint_ using _CSOM_:

    protected void _ConnectSPCSOM(string url, string user, SecureString password)
    {
        using (ClientContext context = new ClientContext(url))
        {
            context.AuthenticationMode = ClientAuthenticationMode.Default;
            context.Credentials = new System.Net.NetworkCredential(user, password);

            try
            {
                Web web = context.Web;
                context.Load(web);
                context.ExecuteQuery();

                _CreateList(context, list);
            }
            catch (Exception exception)
            {
                Console.WriteLine(exception.Message);
            }
        }
    }

Now, let's create another method to handle the creation of our list:

    protected void _CreateList(ClientContext context, string list)
    {
        if (!_CheckIfListExists(context, list))
        {
            _GenerateList(context);
        }

        _AddFields(context);
        _AddData(context);
    }

Here, we call four other methods: one to check if the list already exists, another to generate it if necessary, one to add fields to the list and another one to add the data into the list.

The method that checks if the list already exists could look like so:

    protected bool _CheckIfListExists(ClientContext context, string listName)
    {
        ListCollection listCollection = context.Web.Lists;
        context.Load(listCollection, lists => lists.Include(list => list.Title).Where(list => list.Title == listName));
        context.ExecuteQuery();

        if (listCollection.Count == 0)
        {
            return false;
        }

        return true;
    }

We can now generate the list like so:

    protected void _CreateList(ClientContext context, string list)
    {
        Web web = context.Web;

        ListCreationInformation listCreationInformation = new ListCreationInformation();
        listCreationInformation.Title = list;
        listCreationInformation.TemplateType = (int)ListTemplateType.NoListTemplate;
        List newList = web.Lists.Add(listCreationInformation);
        context.ExecuteQuery();
    }

We can have another version of our method. In fact, sometimes, for example, in the case of a site using the "_Team-Site_" template, we can encounter an error saying that the list template is invalid. In such a case, we have to make a little variation:

    protected void _CreateList(ClientContext context, string list)
    {
        Web web = context.Web;

        ListTemplate listTemplate = web.ListTemplates.GetByName("My template name");
        context.Load(listTemplate);
        context.ExecuteQuery();

        ListCreationInformation listCreationInformation = new ListCreationInformation();
        listCreationInformation.Title = list;
        listCreationInformation.TemplateType = listTemplate.ListTemplateTypeKind;
        List newList = web.Lists.Add(listCreationInformation);
        context.ExecuteQuery();
    }

Now, before we add fields to our list, let's imagine a simple _Struct_ that will help us to handle them:

    public struct ListField
    {
        public string Name { get; set; }
        public string DisplayName { get; set; }
        public string Type { get; set; }
        public string Extra { get; set; }
        public string ChoicesString { get; set; }
        public bool DisplayMainView { get; set; }
        public AddFieldOptions DefaultValue { get; set; }
    }

We can now add our fields like so and check if they already exist:

    protected void _AddFields(ClientContext context, string listName)
    {
        Web web = context.Web;
        List list = web.Lists.GetByTitle(listName);
        context.Load(list);
        context.ExecuteQuery();

        List<ListField> fieldsList = new List<ListField>();
        fieldsList.Add(new ListField() { Name = "Field1", DisplayName = "Field1", Type = "Text", Extra = "", ChoicesString = "", DisplayMainView = true, DefaultValue = AddFieldOptions.DefaultValue });
        fieldsList.Add(new ListField() { Name = "Field2", DisplayName = "Field2", Type = "Text", Extra = "", ChoicesString = "", DisplayMainView = true, DefaultValue = AddFieldOptions.DefaultValue });
        fieldsList.Add(new ListField() { Name = "Field3", DisplayName = "Field3", Type = "Choice", Extra = "Format='Dropdown' FillInChoice='false'", ChoicesString = "<Default>None</Default><CHOICES><CHOICE>None</CHOICE><CHOICE>Choice 1</CHOICE><CHOICE>Choice 2</CHOICE><CHOICE>Choice 3</CHOICE><CHOICE>Choice 4</CHOICE></CHOICES>", DisplayMainView = true, DefaultValue = AddFieldOptions.DefaultValue });

        foreach (ListField field in fieldsList)
        {
            if (_CheckIfFieldExistsInList(field.Name, list))
            {
                continue;
            }

            string str;

            if (field.Type == "Choice")
            {
                str = "<Field Type='" + field.Type + "' DisplayName='" + field.DisplayName + "' Name='" + field.Name + "' " + field.Extra + " >";
                str += field.ChoicesString;
                str += "</Field>";
            }
            else
            {
                str = "<Field Type='" + field.Type + "' DisplayName='" + field.DisplayName + "' Name='" + field.Name + "'/>";
            }

            Field addedField = list.Fields.AddFieldAsXml(str, field.DisplayMainView, field.DefaultValue);
        }

        context.ExecuteQuery();
    }

    protected bool _CheckIfFieldExistsInList(string fieldName, List list)
    {
        foreach (Field field in list.Fields)
        {
            if (field.Title.Equals(fieldName))
            {
                return true;
            }
        }
        return false;
    }

Now it is time to add some data to our list. First, for the sake of our example, let's imagine another really simple _Struct_ that represents that data we want to add. Of course, this will change depending on our situation. Here, it is just for the example.

    public struct WeData
    {
        public string Title { get; set; }
        public string Subtitle { get; set; }
        public string Url { get; set; }
        public string Type { get; set; }
    }

We can now write our method to add or data:

    protected void _AddData(ClientContext context)
    {
        foreach(WebData item in _data)
        {
            ListItem listItem = _CheckIfItemExists(context, item);

            if (listItem == null)
            {
                _InsertItem(context, item);
                continue;
            }

            _UpdateItem(context, item, listItem);
        }
    }

Here, as we can see, we suppose that our data is stocked in a variable named "_data_" which is a list("_List<WebData>_"). Here, we check if each item of this list already exists in our _SharePoint_ list.

    protected ListItem _CheckIfItemExists(ClientContext context, WebData item)
    {
        Web web = context.Web;
        List list = web.Lists.GetByTitle(_targetList);

        CamlQuery query = new CamlQuery
        {
            ViewXml = "<View><Query><Where><Eq><FieldRef Name='Field1' /><Value Type='Text'>" + item.Title + "</Value></Eq></Where></Query></View>"
        };

        ListItemCollection items = list.GetItems(query);
        context.Load(items);
        context.ExecuteQuery();

        if (items.Count == 0)
        {
            return null;
        }

        return items[0];
    }

To query our item and check its existence, we use a _CAML Query_. This check helps us to decide if we have to insert a new item in our _SharePoint_ list or if we have to update an existing one.

    protected void _InsertItem(ClientContext context, WebData item)
    {
        Web web = context.Web;
        List list = web.Lists.GetByTitle(_targetList);

        ListItemCreationInformation itemCreateInfo = new ListItemCreationInformation();
        ListItem listItem = list.AddItem(itemCreateInfo);
        context.ExecuteQuery();

        _SetDataItem(context, item, listItem);
    }

    protected void _UpdateItem(ClientContext context, WebData item, ListItem listItem)
    {
        Web web = context.Web;
        _SetDataItem(context, item, listItem);
    }

Finally, we can update our _SharePoint_ list like so:

    protected void _SetDataItem(ClientContext context, WebData item, ListItem listItem)
    {
        listItem["Title"] = item.Title;
        listItem["Field1"] = item.Subtitle;
        listItem["Field2"] = item.Url;
        listItem["Field3"] = item.Type;
    
        listItem.Update();
        context.ExecuteQuery();
    }

## Conclusion ##

Through this post, we saw how we can work with _SharePoint_ lists using _CSOM_. We had an overview of the different operations we can achieve using the _API_.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!
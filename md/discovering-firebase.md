## [Discovering Firebase](https://mlbors.tumblr.com/post/159302658655/discovering-firebase)

Firebase is a mobile and web application platform that includes a real-time database. Let’s start using it with JS in a really simple manner.

First, we include Firebase in our project by using _[https://www.gstatic.com/firebasejs/3.7.5/firebase.js](https://www.gstatic.com/firebasejs/3.7.5/firebase.js)_

Now, let’s initialize the app!
    
    
    var firebaseConfig = {
        apiKey: "apiKey",
        authDomain: "yourProtectedID.firebaseapp.com",
        databaseURL: "https://databasename.firebaseio.com",
        storageBucket: "yourProtectedID.appspot.com",
        messagingSenderId: "messagingSenderId"
    };
    
    var firebaseApp = firebase.initializeApp(firebaseConfig);
    var firebaseDatabase = firebaseApp.database();

Then, because we’d like to retrieve some data, we create a reference to the query location, in the example, “posts”.
    
    
    var ref = firebaseDatabase.ref("posts");

We can now retrieve data from “posts” like so:
    
    
    ref.on('value', function(data) {
        doSomethingWithData(data.val());
    });

The _on_ method listens for changes permanently. If we need to listen only once, we can use the method _once_ like this:
    
    
    ref.once('value').then(function(data) {
        doSomethingWithData(item);
    });

And now, let’s delete some data!
    
    
    firebaseDatabase.ref("posts").child(item.key).remove();

# explanations
6.3 Talking About the Back End assignment

Explanations
* mongoose:
Mongoose is a bridge between Express / Node and MongoDB. Through Mongoose we can use things similar to Object Oriented Programming to manipulate the data in the database.

* schema:
Schema is kind of like Objects in Javascript. Mongoose uses schema to determine what variables are needed to properly represent it.

* virtual:
Virtual properties are properties that you create to manipulate the data (ex. combine or separate properties), but they won’t get saved to the database.

* model:
Models are like Constructors on Object Oriented Programming. They create new versions based on a schema and model the data from the database into key / value pairs just like constructor objects.

* controller:
The Controller is where everything happens. The routes route to the controller based on the URL pattern and the controller will take the data from the model and display the data to the user.

* routes:
Routes match URL patterns to callback functions that will handle what to do next.

* a pug template:
Pug is a template engine like Handlebars. It takes variables in a static file, replaces them with data from the database and sends it to the client as HTML and displays it to the user.

Render path

http://localhost:3000/catalog/author/58f784654a8f75b43d08bdbc

The URL pattern is recognized by app.js
app.use('/catalog', catalog);

-> calls routes/catalog.js
router.get('/author/:id', author_controller.author_detail); recognizes the URL pattern

-> goes to the controllers/authorController.js

    // Display detail page for a specific Author
    exports.author_detail = function(req, res, next) {

        async.parallel({
            author: function(callback) {
                Author.findById(req.params.id)
                  .exec(callback)
            },
            authors_books: function(callback) {
              Book.find({ 'author': req.params.id },'title summary')
              .exec(callback)
            },
        }, function(err, results) {
            if (err) { return next(err); }
            //Successful, so render

            res.render('author_detail', { title: 'Author Detail', author: results.author, author_books: results.authors_books } );
        });

    };

Does parallel searches through Author and Books (Author and Books come from models/author.js and models/book.js via require where the schemas are created and models used to form key/value pairs with the database data. Also, virtual properties are created.) models by the author’s ID that we get from req.params.id (which came from the URL)

-> Then it renders the results in the template views/author_detail.pug by placing the results into the variables in the template and is displayed to the user. 

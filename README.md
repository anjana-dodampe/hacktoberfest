# Flutter-Layout-Cheat-Sheet
This repo includes identified Flutter Layouts which anyone can refer as their layout cheat sheet.

Basic layout widgets (single child)
So you already know that everything in the Flutter UI is a widget. They're not only structural elements like text and buttons. Layout elements like padding and rows are also widgets. First let's take a look at some of the most common layout widgets, and later we will see how to combine them into more complex layouts.

Padding
Thinking of padding as a widget is strange when you first come to Flutter. At least in iOS and Android world, padding is a parameter. But in Flutter, if you want add some extra space around a widget, then you wrap it in a Padding widget.

If you run the boilerplate code from the Setup section above, you should see something like this:



Now to add padding, wrap the Text widget with a Padding widget. In Android Studio this can be accomplished by placing your cursor on the widget and pressing Option+Enter (or Alt+Enter in Windows/Linux).



which gives you this:

    Widget myLayoutWidget() {
      return Padding(
        padding: EdgeInsets.all(8.0),
        child: Text("Hello world!"),
      );
    }
The EdgeInsets parameter is used to specify the amount of padding. Here all was used to indicate that every side (left, top, right, and bottom) should have equal padding. If you want them to have different values, then you can use only instead of all.

Hot reload the app and you should have this:



Notice that now the text has moved away from the edges. It has a padding of 8.0 logical pixels all around it.

Experiment yourself:

Change the padding value
Make the top padding be different than the right padding
Alignment
To center a widget, the concept is the same as it was for padding. This time you just wrap your widget with a Center widget. You can type it out or there is a shortcut menu option for it, too.



In addition to centering it, I added some styling to the Text widget so that the font size is more visible. If you are going the cut-and-paste route, then replace the myLayoutWidget() method with the following code:

    Widget myLayoutWidget() {
      return Center(
        child: Text(
          "Hello world!",
          style: TextStyle(fontSize: 30),
        ),
      );
    }
Hot reload your app and you should see the text centered in middle of the screen.



What about if you want to align a widget somewhere else? You can use the Align widget for that. You can either pass in relative x and y values or you can use the predefined ones. Here are the options. The items in the same row are equivalent.

Alignment.topLeft	Alignment(-1.0, -1.0)
Alignment.topCenter	Alignment(0.0, -1.0)
Alignment.topRight	Alignment(1.0, -1.0)
Alignment.centerLeft	Alignment(-1.0, 0.0)
**Alignment.center**	**Alignment(0.0, 0.0)**
Alignment.centerRight	Alignment(1.0, 0.0)
Alignment.bottomLeft	Alignment(-1.0, 1.0)
Alignment.bottomCenter	Alignment(0.0, 1.0)
Alignment.bottomRight	Alignment(1.0, 1.0)
You can see that another way to center something is to use Alignment.center or Alignment(0.0, 0.0). Actually, the Center widget is just a special case of the Align widget.

The following is an image I made for this Stack Overflow answer. (Check that answer out for even more details on alignment.)



Notice the (1,2) position in the bottom right. This shows that you can even align something outside of the parent. Remember that these numbers are relative to the width and height of the parent widget.

Your turn. Paste in the following code.

    Widget myLayoutWidget() {
      return Align(
        alignment: Alignment.topCenter,
        child: Text(
          "Hello",
          style: TextStyle(fontSize: 30),
        ),
      );
    }
Now adjust the Alignment to try to get the text to move everywhere you can see "Hello" in the following image (but just one location at a time).



Did you get the blue one, too? For that you use Alignment(0.5, 0.5).

Container
We already met the Container widget in the last lesson. It is a combination of several simpler widgets. In addition to having Padding and Align built in, it also has a DecoratedBox (for background color, border, and more) and a ConstrainedBox (for size constraints).

Plug this code in:

    Widget myLayoutWidget() {
      return Container(
        margin: EdgeInsets.all(30.0),
        padding: EdgeInsets.all(10.0),
        alignment: Alignment.topCenter,
        width: 200,
        height: 100,
        decoration: BoxDecoration(
          color: Colors.green,
          border: Border.all(),
        ),
        child: Text("Hello", style: TextStyle(fontSize: 30)),
      );
    }


Play around with the parameters and see how adjusting them affects how the widgets look. Notice the margin parameter. Margin means the spacing outside of the border, while padding is the spacing inside of the border. Technically speaking, though, there is no such thing as margin in Flutter. Under the hood it is just another Padding widget that wraps the DecoratedBox.

Note: Flutter is open source and well documented. You can learn a lot about how widgets are built if you explore the source code. In Android Studio Command + click (or Ctrl + click in Windows/Linux) on the widget name to view its source code.

Basic layout widgets (multiple children)
The widgets above only took one child. When creating a layout, though, it is often necessary to arrange multiple widgets together. We will see how to do that using rows, columns, and stacks.

Rows and columns
Rows are easy. Just pass in a list of widgets to Row's children parameter.

    Widget myLayoutWidget() {
      return Row(
        children: [
          Icon(Icons.home),
          Icon(Icons.home),
          Icon(Icons.home),
          Icon(Icons.home),
        ],
      );
    }
which gives



Now replace Row with Column and you get



What if you want to make the contents of the row or column be evenly spaced across the screen? Then wrap each child with an Expanded widget.

    Widget myLayoutWidget() {
      return Row(
        children: [
          Expanded(child: Icon(Icons.home)),
          Expanded(child: Icon(Icons.home)),
          Expanded(child: Icon(Icons.home)),
          Expanded(child: Icon(Icons.home)),
        ],
      );
    }


The Expanded widget can take a flex parameter. This is useful for giving size weights to the children. For example, here are two Containers in a row. The first one takes up 70% of the row and the second one takes up 30%.

    Widget myLayoutWidget() {
      return Row(
        children: [
          Expanded(
            flex: 7,
            child: Container(
              color: Colors.green,
            ),
          ),
          Expanded(
            flex: 3,
            child: Container(
              color: Colors.yellow,
            ),
          ),
        ],
      );
    }


Notes:

If you wanted a single view to only take a fraction of its parent's size, then you could use a FractionallySizedBox.
Check out Flutter — Row/Column Cheat Sheet for much, much more.
Stacks
The Stack widget lays out its children like a stack of pancakes. You set it up like the Row and Column widgets. Whichever child comes first is the one on the bottom.

You could do something like this:

    Widget myLayoutWidget() {
      return Stack(
        children: [
          Icon(Icons.home),
          Icon(Icons.home),
          Icon(Icons.home),
          Icon(Icons.home),
        ],
      );
    }
but who needs four icons stacked on top of each other?



A more likely scenario is to use a stack to write text on an image. Let's take this image



and put it in our project:

Create an images folder in the root of your project and copy the sheep.jpg file into it.
Register images as an assets folder in your pubspec.yaml file.
    flutter:
      assets:
        - images/
(If that didn't make sense, see this post for more details.)

Now we can get the image in code and use a Stack widget to display some text over it.

    Widget myLayoutWidget() {
      return Stack(

        // any unpositioned children (ie, our text) will be aligned at the bottom right
        alignment: Alignment.bottomRight,

        children: [

          // first child in the stack is on bottom
          Image.asset('images/sheep.jpg'), //            <--- image

          // second child in the stack
          Padding(
            padding: EdgeInsets.all(16.0),
            child: Text(
              'Baaaaaa', //                              <--- text
              style: TextStyle(fontSize: 30),
            ),
          ),

        ],
      );
    }
Do a full restart rather than a hot reload.



So the take away is that any time you need overlapping widgets, use a Stack to lay them out. (That's not always the case, but you can take it as a general rule.)

Other layout widgets
We don't have space to cover all of the layout widgets here, but you have seen the most important ones already. Here are a few others that deserve mentioning:

ListView: This widget scrolls rows or columns of content that is too big to fit on the screen. We saw this in our last lesson on widgets.
GridView: This widget scrolls content that is laid out in a grid of rows and columns.
Scaffold: This is a widget provided by the Material package. It gives an easy way to add an AppBar, FloatingActionButton, Drawer, BottomNavigationBar, SnackBar, and more. Look at your main.dart file and you'll see that we are using a Scaffold widget already.
Building complex layouts
Since you already know how to use the simple layout widgets that we talked about above, there really isn't anything hard about building complex layouts. The trick is just to break the complex layout into smaller simple layouts. Rows and columns are your friends here.

As an example, lets take this image from the Pusher website and duplicate its layout:



How can we convert this into simple rows and columns? First notice that it can be divided into a single column with three rows.



The first and the third rows both have two simple items: an image and a text string.



We now have enough information to build our complex layout widget. Before you look at the code below, try to build it yourself using what we have already learned. Instead of the Pusher Beams icon, you can use another placeholder icon or image. I’ll use a 💚.

Got it? Need a hint? Here is a rough outline that implements the structure that we saw in the image above:

    Widget myLayoutWidget() {
      return Column(
        children: [
          Row(
            children: [
              Icon(Icons.favorite),
              Text('BEAMS'),
            ],
          ),
          Text('description...'),
          Row(
            children: [
              Text('EXPLORE BEAMS'),
              Icon(Icons.arrow_forward),
            ],
          ),
        ],
      );
    }


It needs some work, but you can see that we have the correct structure. Now make it look good by adding padding, alignment, and color. Try to do it yourself before you look at my code below.

Really. Don't look yet. Not until you try it yourself. I'm serious.

OK, ready?

    Widget myLayoutWidget() {

      // wrap everything in a purple container
      return Container(
        margin: EdgeInsets.all(16.0),
        padding: EdgeInsets.all(16.0),
        decoration: BoxDecoration(
          color: Colors.purple[900],
          border: Border.all(),
          borderRadius: BorderRadius.all(Radius.circular(3.0)),
        ),

        // column of three rows
        child: Column(

          // this makes the column height hug its content
          mainAxisSize: MainAxisSize.min,
          children: [

            // first row
            Row(
              children: [
                Padding(
                  padding: EdgeInsets.only(right: 8.0),
                  child: Icon(Icons.favorite,
                    color: Colors.green,
                  ),
                ),
                Text(
                    'BEAMS',
                  style: TextStyle(
                    color: Colors.white,
                  ),
                ),
              ],
            ),

            // second row (single item)
            Padding(
              padding: EdgeInsets.symmetric(
                vertical: 16.0,
                horizontal: 0,
              ),
              child: Text('Send programmable push notifications to iOS and Android devices with delivery and open rate tracking built in.',
                style: TextStyle(
                  color: Colors.white,
                ),
              ),
            ),

            // third row
            Row(
              children: [
                Text('EXPLORE BEAMS',
                  style: TextStyle(
                    color: Colors.green,
                  ),
                ),
                Padding(
                  padding: EdgeInsets.only(left: 8.0),
                  child: Icon(Icons.arrow_forward,
                    color: Colors.green,
                  ),
                ),
              ],
            ),

          ],
        ),
      );
    }
Ugh, look at that code! I've made the terrible mountain and valley indentations that I was complaining about last time. We'll get back to that. Right now take a look at the result:



It's not perfect, but it's not bad, either. I'm happy with it.

Making complex layouts readable
All the indentation in the code above makes it hard to read. The solution to this is to break the large code block into smaller chunks. There are a few ways to do this.

Break sections out as variables
In the abbreviated code below the rows have been extracted from the bigger widget into variables.

    Widget myLayoutWidget() {

      Widget firstRow = Row(
        ...
      );
      Widget secondRow = ...
      Widget thirdRow = ...

      return Container(
        ...
        child: Column(
          children: [
            firstRow,
            secondRow,
            thirdRow,
          ],
        ),
      );
    }
Break sections out as functions
This is basically the same as above, except this time the Row is a function instead of a variable. This is how we set up the boilerplate code at the beginning of the lesson with the myLayoutWidget() function.

Here is what firstRow() would look like:

    Widget firstRow() {
      return Row(
        children: [
          Padding(
            padding: EdgeInsets.only(right: 8.0),
            child: Icon(Icons.favorite,
              color: Colors.green,
            ),
          ),
          Text(
            'BEAMS',
            style: TextStyle(
              color: Colors.white,
            ),
          ),
        ],
      );
    }
It is called like this:

        ...
        child: Column(
          children: [
            firstRow(),
            secondRow(),
            thirdRow(),
          ],
        ...
Break sections out as widgets
The MyApp widget in the boilerplate code at the beginning of this lesson is an example of creating a custom widget.

Here is the first row extracted as its own widget.

    class FirstRow extends StatelessWidget {
      // the build method is required when creating a StatelessWidget
      @override
      Widget build(BuildContext context) {
        return Row(
          children: [
            Padding(
              padding: EdgeInsets.only(right: 8.0),
              child: Icon(Icons.favorite,
                color: Colors.green,
              ),
            ),
            Text(
              'BEAMS',
              style: TextStyle(
                color: Colors.white,
              ),
            ),
          ],
        );
      }
    }
The widget is created like this:

        ...
        child: Column(
          children: [
            FirstRow(),
            SecondRow(),
            ThirdRow(),
          ],
        ...
Note: You may have seen the new keyword used in examples around the Internet. As of Dart 2, though, new is no longer required when creating an object.

Tools
Flutter has a few builtin tools for helping you debug layouts.

Flutter Inspector
In Android Studio you can find the Flutter Inspector tab on the far right. Here we see our layout as a widget tree.



Visual rendering
You can turn on visual rendering by setting debugPaintSizeEnabled to true in your main() function.

In your main.dart file replace this line

    void main() => runApp(MyApp());
with this

    // add this line to your imports
    import 'package:flutter/rendering.dart';

    // update your main() function
    void main() {
      debugPaintSizeEnabled = true; //         <--- enable visual rendering
      runApp(MyApp());
    }
This outlines your widgets with blue in the emulator. You will need to do a full restart of your app rather than a hot reload to see it take effect.



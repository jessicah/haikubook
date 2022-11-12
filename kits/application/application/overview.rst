BApplication
============

The BApplication class defines an object that represents your application,
creates a connection to the App Server, and runs your app's main message loop.
An app can only create one BApplication object; the system automatically sets
the global ``be_app`` object to point to the ``BApplication`` object you create.

A ``BApplication`` object's most pervasive task is to handle messages that are
sent to your app, a subject that's described in detail below. But message
handling aside, you can also use your ``BApplication`` object to...

Control the cursor.
	``BApplication`` defines functions that hide and show the cursor, and set
	the cursor's image. See ``SetCursor()``.

Access the window list.
	You can iterate through the windows that your application has created with
	``WindowAt()``.

Get information about your application.
	Your app's signature, executable location, and launch flags can be retrieved
	through ``GetAppInfo()``. Additional information icons, version strings,
	recognized file types can be retrieved by creating an ``BAppFileInfo``
	object based on your app's executable file. ``BAppFileInfo`` is defined in
	``The Storage Kit``.

``be_app`` and Subclassing BApplication
=======================================

Because of its importance, the ``BApplication`` object that you create is
automatically assigned to the global ``be_app`` variable. Anytime you need to
refer to your ``BApplication`` object from anywhere in your code you can use
``be_app`` instead.

Unless you're creating a very simple application, you should subclass
``BApplication``. But be aware that the ``be_app`` variable is typed as
``BApplication *``. You'll have to cast ``be_app`` when you call a function
that's declared by your subclass::

	((MyApp *)be_app)->MyAppFunction();

Constructing the Object and Running the Message Loop
====================================================

As with all ``BLooper``s, to use a ``BApplication`` you construct the object and
then tell it to start its message loop by calling the ``Run()`` function.
However, unlike other loopers, ``BApplication``s ``Run()`` doesn't return until
the application is told to quit. And after ``Run()`` returns, you delete the
object, it isn't deleted for you.

Typically, you create your ``BApplication`` object in your ``main()``
function--it's usually the first object you create. The barest outline of a
typical ``main()`` function looks something like this::

	#include <Application.h>

	main()
	{
		new BApplication("application/x-vnd.your-app-sig");
		/* Further initialization goes here -- read settings,
		   set globals, etc. */
		be_app->Run();
		/* Clean up -- write settings, etc. */
		delete be_app;
	}

#. The ``main()`` function doesn't declare ``argc`` and ``argv`` parameters
	(used for passing along command line arguments). If the user passes command line
	arguments to your app, they'll show up in the ``ArgsReceived()`` hook function.
#. Why no pointer assignment? The constructor automatically assigns the object
	to ``be_app``, so you don't have to assign it yourself.
#. The string passed to the constructor sets the application's signature. This
	is a precautionary measure; it's better to add the signature as a resource
	than to define it here (a resource signature overrides the constructor
	signature). Use the FileTypes app to set the signature as a resource.

Other Topics
============

Locking
	Like a ``BLooper``, a ``BApplication`` must be locked before calling certain
	protected functions. The ``BApplication`` locking mechanism is inherited
	without modification from ``BLooper``.

FileTypes settings
	The ``BApplication`` object represents your application at runt-time.
	However, some of the characteristics of your application, whether it can be
	launched more than once, the file types it can open, its icon are not
	run-time decisions.

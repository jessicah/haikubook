.. cpp:class::	BDeskbar

BDeskbar
========

Constructor and Destructor
--------------------------

BDeskbar()
	::

		BDeskbar();

	The constructor creates and returns a new :cpp:class:`BDeskbar` object.

~BDeskbar()
	::

		BDeskbar();

	Deletes the :cpp:class:`BDeskbar` object.

Member Functions
----------------

.. cpp:function::
	status_t BDeskbar::AddItem()
	status_t BDeskbar::RemoveItem()
	status_t BDeskbar::CountItems()
	status_t BDeskbar::HasItems()
	status_t BDeskbar::GetItemInfo()

AddItem(), RemoveItem(), CountItems(), HasItem(), GetItemInfo()
	::

		status_t AddItem(BView* view,
		                 int32* id = NULL);
		status_t AddItem(entry_ref* addon,
		                 int32* id = NULL);

	::

		status_t RemoveItem(int32 id);
		status_t RemoveItem(const char* name);

	::

		int32 CountItems() const;

	::

		bool HasItem(int32 id) const;
		bool HasItem(const char* name) const;

	::

		status_t GetItemInfo(int32 for_id,
							const char** found_name);
		status_t GetItemInfo(const char* for_name,
							const int32* found_id);

	.. warning::

		A view that's sitting on the Deskbar's shelf may not call any of these functions.

	The Deskbar contains a shelf that contains replicants (archivable
	:cpp:class:`BView`s). Typically, these replicants monitor or control some
	service. For example, the BeOS provides shelf items that monitor and control the
	input method mechanism, PPP, and the mail daemon (the date/time view is not a
	shelf replicant).

	<<image of the ProcessController replicant>>

	:any:`AddItem() <BDeskbar::AddItem()>` puts a new item on the Deskbar's shelf
	:cpp:expr:`view`, the :cpp:class:`BView` that will be displayed on the shelf,
	must be archivable (see :cpp:class:`BArchivable`). An item on the shelf is
	identified by name and an integer id. The name is that of the view itself (i.e.,
	as assigned in the :cpp:class:`BView` constructor); the id is generated by the
	Deskbar and is guaranteed to be unique. :cpp:expr:`id`, if supplied, is set to
	the added item's unique id number.

	You can also add an item to the Deskbar by passing an entry_ref,
	:cpp:expr:`addon`, to the Deskbar add-on to place there.

	:any:`RemoveItem() <BDeskbar::RemoveItem()>` removes the shelf item identified by
	:cpp:expr:`name` or :cpp:expr:`id`.

	:any:`CountItems() <BDeskbar::CountItems()>` returns the number of items currently on the shelf
	(keep in mind that it doesn't count the date/time view).

	:any:`HasItem() <BDeskbar::HasItem()>` returns :cpp:enum:`true` if the Deskbar shelf contains
	the item identified by :cpp:expr:`name` or :cpp:expr:`id`.

	:cpp:func:`GetItemInfo() <BDeskbar::GetItemInfo()>` points :cpp:expr:`found_name` to the name of the
	item identified by :cpp:expr:`for_id`, or sets :cpp:expr:`found_id` of the item
	identified by :cpp:expr:`for_name`.

	.. warning::

		The caller is responsbile for freeing :cpp:expr:`found_name`.

	.. |return_code_ok| replace:: The request to add, remove, or get info on the
		item was successfully communicated to the Deskbar. Note that this doesn't
		mean that the function actually did what it was supposed to do.

	.. |return_code_bad_value| replace:: :cpp:expr:`GetItemInfo()`
		:cpp:expr:`found_name` is :cpp:enum:`NULL`.

	+-------------------------+----------------------------------+
	| Return Code             | Description                      |
	+=========================+==================================+
	| :cpp:enum:`B_OK`        | |return_code_ok|                 |
	+-------------------------+----------------------------------+
	| :cpp:enum:`B_BAD_VALUE` | |return_code_bad_value|          |
	+-------------------------+----------------------------------+
	| Negative values         | A message-sending error occurred |
	+-------------------------+----------------------------------+

Frame()
	::

		:cpp:class:`BRect` Frame() const;

	Returns the Deskbar's frame in screen coordinates.

Location(), IsExpanded(), SetLocation(), Expand()
	::

		deskbar_location Location(bool* isExpanded = NULL) const;

	::

		bool IsExpanded() const;

	::

		status_t SetLocation(deskbar_location location)
							bool isExpanded = false);

	::

		status_t Expand(bool expand);

	:cpp:function:`Location()` returns a symbolic description of the Deskbar's
	current location; see :cpp:expr:`deskbar_location` for the list of pre-defined
	locations. :cpp:expr:`isExpanded` (if supplied) is set to :cpp:enum:`true` if
	the Deskbar is expanded, and :cpp:enum:`false` if it's contracted;
	:cpp:function:`IsExpanded()` returns the expansion value directly. Expansion and
	contraction is variable only if the Deskbar's location is left-top or right-top;
	for all other locations, the expansion state is hard-wired. See
	:cpp:expr:`deskbar_location` for illustrations.

	:cpp:function:`SetLocation()` sets the Deskbar's location and expands/contracts
	the Deskbar; for some locations, the expansion/contraction is hard-wired.
	:cpp:function:`Expand()` expands/contracts the Deskbar (if the setting isn't
	hard-wired) without setting its location. You should very rarely need to call
	these functions. Moving and expanding the Deskbar is in the user's domain.

	:cpp:function:`SetLocation()` and :cpp:function:`Expand()` return...

	.. |return_code_o2| replace:: The new location or expansion request was
		successfully communicated to the Deskbar. Whether the parameters were
		actually enforced isn't indicated.

	.. |return_code_other| replace:: The Deskbar isn't running, or some other
		message-sending error occurred.

	+------------------+---------------------+
	| Return Code      | Description         |
	+==================+=====================+
	| :cpp:enum:`B_OK` | |return_code_o2|    |
	+------------------+---------------------+
	| Negative values  | |return_code_other| |
	+------------------+---------------------+

Deskbar Types
-------------

Deskbar Location
	::

		enum deskbar_location;

	+------------------------------------+--------------------------------------------------+
	| Constant                           | Description                                      |
	+====================================+==================================================+
	| :cpp:enum:`B_DESKBAR_TOP`          | Expanded (only) along the top.                   |
	+------------------------------------+--------------------------------------------------+
	| :cpp:enum:`B_DESKBAR_BOTTOM`       | Expanded (only) along the bottom.                |
	+------------------------------------+--------------------------------------------------+
	| :cpp:enum:`B_DESKBAR_LEFT_BOTTOM`  | Contracted (only) in the bottom left corner      |
	+------------------------------------+--------------------------------------------------+
	| :cpp:enum:`B_DESKBAR_RIGHT_BOTTOM` | Contracted (only) in the bottom right corner     |
	+------------------------------------+--------------------------------------------------+
	| :cpp:enum:`B_DESKBAR_LEFT_TOP`     | In the top left corner (expanded or contracted)  |
	+------------------------------------+--------------------------------------------------+
	| :cpp:enum:`B_DESKBAR_RIGHT_TOP`    | In the top right corner (expanded or contracted) |
	+------------------------------------+--------------------------------------------------+

	The :cpp:expr:`deskbar_location` constants are used to set and return the
	Deskbar's location (see :cpp:function:`Location()`). The six locations are shown
	in the two illustrations below:

	<<image 1>>

	<<image 2>>

	The :cpp:expr:`deskbar_location` value affects the Deskbar's expanded state: The
	Deskbar can be expanded or contracted in a :cpp:enum:`B_DESKBAR_LEFT_TOP` and
	:cpp:enum:`B_DESKTOP_RIGHT_TOP` locations only. In the other locations, the
	expansion/contraction is hard-wired. The illustration below shows a left-top
	Deskbar in its expanded and contracted states:

	<<image 3>>

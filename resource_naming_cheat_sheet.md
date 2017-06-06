# ANDROID RESOURCE NAMING CHEAT SHEET

## **Format:** \<WHAT>\_\<WHERE>\_\<DESCRIPTION>\_\<SIZE>


| PlaceHold | Meaning |
| :---: | :--- |
| What | Indicate what the resource actually represents. Fixed set of options per resource type. |
| Where | Custom part, describe where it logically belongs in the app. <br/> Resources used in multiple screens or throughout the app use **all**.
| Description | Differentiate multiple elements in one android component. |
| Size | Either a precise size or size bucket. Optionally used for drawables and dimensions. |

1. ### Layouts: \<WHAT>\_\<WHERE>\.xml

| Prefix | Usage | Example |
| :---: | :--- | :--- |
| *activity* | contentview for activity | **_activity_main_**: content view of the MainActivity
| *fragment* | view for a fragment | **_fragment_articledetail_**: view for the ArticleDetailFragment
| *view* | inflated by a custom view | **_view_menu_**: layout inflated by custom view class MenuView
| *item* | layout used in list, recycler, gridview | **_item_article_**: list item in ArticleRecyclerView
| *layout* | layout reused using the include tag | **_layout_actionbar_backbutton_**: layout for an actionbar with a backbutton

1. ### Strings: \<WHERE>\_\<DESCRIPTION>\.xml

    **Examples:**

    * **_articledetail_title_**: title of ArticleDetailFragment
    * **_feedback_explanation_**: feedback explanation in FeedbackFragment
    * **_feedback_namehint_**: hint of name field in FeedbackFragment
    * **_all_done_**: generic “done” string

1. ### Drawables: \<WHERE>\_\<DESCRIPTION>\_\<SIZE>\.xml

    **Examples:**

    * **_articledetail_placeholder_**: placeholder in ArticleDetailFragment
    * **_all_infoicon_**: generic info icon
    * **_all_infoicon_large_**: large version of generic info icon
    * **_all_infoicon_24dp_**: 24dp version of generic info icon

1. ### IDs: \<WHAT>\_\<WHERE>\_\<DESCRIPTION>\.xml

    **\<WHAT>** is the class name of the xml element it belongs to.

    **\<WHERE>** is the screen the ID is in.

    **\<DESCRIPTION>** is optional, to distinguish similar elements in one screen.

    **Examples:**

    * **_articledetail_placeholder_**: placeholder in ArticleDetailFragment
    * **_all_infoicon_**: generic info icon
    * **_all_infoicon_large_**: large version of generic info icon
    * **_all_infoicon_24dp_**: 24dp version of generic info icon

1. ### Dimensions: \<WHAT>\_\<WHERE>\_\<DESCRIPTION>\_\<SIZE>\.xml

    Apps should only define a limited set of dimensions, which are constantly reused. This makes most dimensions all by default.

| Prefix | Usage | Example |
| :---: | :--- | :--- |
| width | width in dp | **_height_menu_profileimage_**: height of profile image in menu
| height | height in dp | **_height_toolbar_**: height of all toolbars
| size | if width == height | **_size_menu_icon_**: size of icons in menu
| margin | margin in dp |
| padding | padding in dp |
| elevation | elevation in dp |
| keyline | absolute keyline measured from view edge in dp | **_keyline_listtext_**: listitem text is aligned at this keyline
| textsize | size of text in sp | **_textsize_medium_**: medium size of all text

## Advantages

1. Ordering of resources by screen

    The WHERE part describes what screen a resource belongs to. Hence it is easy to get all IDs, drawables, dimensions,… for a particular screen.

1. Strongly typed resource IDs

    For resource IDs, the WHAT describes the class name of the xml element it belongs to. This makes is easy to what to cast your findViewById() calls to.

1. Better resource organizing

    File browsers/project navigator usually sort files alphabetically. This means layouts and drawables are grouped by their WHAT (activity, fragment,..) and WHERE prefix respectively. A simple Android Studio plugin/feature can now display these resources as if they were in their own folder.

1. More efficient autocomplete

    Because resource names are far more predictable, using the IDE’s autocomplete becomes even easier. Usually entering the WHAT or WHERE is sufficient to narrow autocomplete down to a limited set of options.

1. No more name conflicts

    Similar resources in different screens are either all or have a different WHERE. A fixed naming scheme avoids all naming collisions.

1. Cleaner resource names

    Overall all resources will be named more logical, causing a cleaner Android project.

1. Tools support

    This naming scheme could be easily supported by the Android Studio offering features such as: lint rules to enforce these names, refactoring support when you change a WHAT or WHERE, better resource visualisation in project view,…

## Known limitations

1. Screens need to have unique names

    To avoid collisions in the <WHERE> argument, View (like) classes must have unique names. Therefore you cannot have a “MainActivity” and a “MainFragment”, because the “Main” prefix would no longer uniquely identify one <WHERE>.

1. Refactoring not supported

    Changing class names does not change along resource names when refactoring. So if you rename “MainActivity” to “ContentActivity”, the layout “activity_main” won’t be renamed to “activity_content”. Hopefully Android Studio will one day add support for this.

1. Not all resource types supported

    The proposed scheme currently does not yet support all resource types. For some resources this is because they are less frequently used and tend to be very varied (e.g. raw and assets). For other resources this is because they are a lot harder to generalize (e.g. themes/styles/colors/animations).

    Your apps colors palette likely wants to reuse the terminology of your design philosophy. Animations can range from modest (fade) to very exotic. Themes and styles already have a naming scheme that allows you to implicitly inherit properties.

# Creating and Managing Pages

You've successfully created a site for your Liferay instance, but you may need
to add new pages or edit existing pages. From the Site Administration &rarr;
*Pages* section of the Product Menu, your site pages can be accessed and
configured. If you're not currently on the site you'd like to edit, click the
*Compass* icon (![Compass](../../../images/icon-compass.png)) next to your
current site name in the Product Menu and select your desired site. The edits
you make to your pages are only made to the scope you've selected in the Product
Menu.

![Figure 1: The Pages menu allows you to edit your site pages as a whole.](../../../images/managing-site-pages.png)

To add new pages to your site, click the *Add Page* icon (![Add
Page](../../../images/icon-control-menu-add.png)) for the page or page set you'd
like to add a page to. You can manage a plethora of options from the Pages menu
including page name, site layout, and site template.

There are two ways to edit a page: you can select the *Edit Page* option
(![Edit Page](../../../images/icon-control-menu-gear.png)) for any page in your
site from the Pages menu or you can edit the specific page you're currently
viewing by selecting the *Edit Page* option
(![Edit Page](../../../images/icon-control-menu-gear.png)) from the top menu.
The latter option will only edit the specific page you're currently viewing.

![Figure 2: The *Edit Page* interface allows you to edit the current page you're viewing.](../../../images/web-content-managing-single-page.png)

In summary, the Pages menu is used to navigate to pages, create new pages, and
edit existing pages. Note that you can also switch to managing a set of pages in
the Pages menu. Click on *Public Pages* or *Private Pages* to manage that page
group.

Liferay's page groups are always associated with sites. Even users' personal
pages are part of their personal sites. All pages belong to one of two types of
page sets: public pages and private pages. By default, public pages are
accessible to anyone, even non-logged in users (guests). Private pages are
accessible only to users who are members of the site which owns the pages. This
means the private pages of an organization's site would only be viewable by
site members and members of the organization. 

Regardless of whether the pages are public or private, Liferay uses the same
interface to manage them. You'll look at this interface more closely next. 

## Creating Pages

From the Pages menu in the Product Menu, you can add a page or child page to any
existing page or page set by clicking the *Add Page* button. Because *Public
Pages* is selected on the left, clicking *Add Page* here adds a top level page
making it a sibling page of the Welcome page. You can, however, nest pages as
deeply as you like. To create a sub-page under the Welcome page, select the *Add
Child Page* button next to *Welcome*.

The *Add Child Page* lets you create child pages underneath the page you've
selected. You can nest pages as deep as you like but for every page below the
top level hierarchy you must provide navigation to it via a Navigation or
Breadcrumb portlet, at least with most themes (including the default).
Developers can create themes which have cascading menu bars which show the full
hierarchy. Some examples of that are in Liferay's plugin repositories.

Once you've selected the *Add Page* or *Add Child Page* button, you're directed
to an *Add New Page* interface that lets you configure the page to your liking.
If you later decide you don't like the order of your pages, you can drag and
drop them in the list to put them in whatever order you want. Go ahead and add
another top level page and name it *Community*. You'll use this page for the
Recent Bloggers and Wiki applications.

![Figure 3: You can add a page to your site by giving it a name, page template, and page type.](../../../images/web-content-add-page.png)

When you create a new page, you can create either a blank page or a page
prepopulated with apps from a page template. When you're entering the name
of the page, you can select from a list of page templates that are currently
available. You can learn more about Page Templates and how to use them in the
[Creating Pages from Templates](/discover/portal/-/knowledge_base/7-0/creating-pages-from-templates) 
section. To view the page you've added, click the page name from the left panel.
By default, all pages are created as an empty page but in some situations, you
might want to use one of the other options. You can take a look at all the page
options you have below, along with their respective descriptions.

**Empty Page:** the default pages that are used. They have a layout which you
can drag and drop portlets into. This page type creates an empty page with
configurable column sizes.

**Content Display Page:** are used to search available content, explore related
content with tags, and browse content catagories. This is a specialized page
dedicated to managing web content from a page.

**Wiki:** display content related to wikis. This is a specialized page
dedicated to creating, managing, and viewing wiki articles from a page.

**Blog:** display content related to blogs. This is a specialized page dedicated
to creating, editing, and viewing blogs.

**Page Set:** exists as a container that holds child pages. These pages are not
intended to hold content. Because the page holds no content and has no purpose
but to hold other child pages, there is no friendly URL for this page. It also
cannot serve as a landing page for a site.

**Panel:** can have any number of portlets on them, as selected by an
administrator, but only one will be displayed at a time. Users select which
portlet they want to use from a menu on the left side of the page and the
selected portlet takes up the entire page. 

**Link to URL:** are redirects to any URL specified by an administrator. You can
use URL pages to create links to pages belonging to other sites of your Liferay
instance or to pages of an external site. Use URL pages cautiously since blind
redirects create a poor user experience.

**Embedded:** display content from another website inside of your instance.
An administrator can set a URL from in the page management interface and that
page will appear in the context and within the navigation of your Liferay
instance.

**Full Page Application:** creates a page with one column that displays a single
full page application.

**Link to a Page of This Site:** creates a page which functions as an immediate
redirect to another page within the same site. You can select which page to link
to from a dropdown in the page management interface. You could use a *Link to a
Page of This Site* to place a deeply nested page in the primary navigation menu
of your site, for example.

**Copy of a Page of This Site:** displays a copy of a pre-existing page in your
Liferay instance.

Now that you know the basics of adding site pages, you can start working on the
Lunar Resort site. If you're not currently on the site you're interested in
adding pages to, navigate to Site Administration in the Product Menu, select the
compass icon next to the current site name, and select the site you wish to
edit.

If you're creating a site with many pages and sub-pages, the side panel view may
seem constricted. In the Pages menu, you have the option to expand the Pages
view to allow for more room to work. Select the *Expand* icon
(![Expand](../../../images/icon-expand.png)) to see a full view of your page
hierarchy.

![Figure 4: Expanding your page view gives you much more room to visualize your page hierarchy.](../../../images/expand-page-hierarchy.png)

As stated previously, if you ever need to modify the page you've created for
your site, select the *Edit* icon
(![Edit Page](../../../images/icon-control-menu-gear.png)) either from the Pages
menu in the Product Menu or the top Control Menu on the currently viewed page.
When configuring a specific page, you're given more options than when you were
creating a new page. These extended options for configuring specific pages are
covered in more detail later in this section.

There are also configuration options that are only available for either
indiviual pages or page groups only. You'll learn about options available for
both instances.

Navigate to the Pages menu and select the gear icon to edit the Public Pages
group. You should be able to define and manage pages in Liferay at this point so
next, you'll look at the options you have available for configuring your public
or private pages as a whole, and the individual pages residing within those
groups.

### Customizing the Look and Feel of Site Pages

When you select *Edit Public Pages*, it defaults to the Look and Feel tab. On
this tab, you're presented with an interface that allows you to choose a theme
for the current site. Themes can transform the entire look of the portal. They
are created by developers and are easily installed using the Liferay
Marketplace. Since we don't have any themes beyond the default one installed
yet, we'll use the default theme for our pages.

<!-- TODO: Add image of Look and Feel section here when more stable for 7.0:
[Figure 2.8: The Look and Feel interface allows you to choose a theme for the current site.](../../../images/04-look-and-feel.png)
-->

Many themes include more than one color scheme. This allows you to keep the
existing look and feel while giving your site a different flavor. Change the
color scheme from blue to green by selecting *Dark* under *Color Schemes*. If
you now go back to the site (by clicking the *Go to Site* button located under
the Site Administration dropdown menu), you'll see some parts of the page are
now tinged with a darker hue.

<!-- TODO: Color change may differ for 7.0. Check when Color Schemes are
reintroduced. -Cody -->

If you apply a color scheme to a set of public or private pages, it is, by
default, applied to each page in the set. If, however, you click the *Edit Page*
button for a specific page, you can select *Define a specific look and feel for
this page* from the *Look and Feel* tab to make the color scheme apply to this
page only. You can use this feature to choose a different color scheme for a
particular page than the one defined for the set of public or private pages to
which it belongs.

<!-- TODO: The Color Schemes option is not currently available for the default
theme. Checked with Julio and he said color schemes are still available, but not
for the current default theme. He's requesting different color schemes be added
before the 7.0 release so the options are available by default (instead of only
with custom themes. Therefore, I'm leaving the docs here assuming this gets
finished. Need to confirm -Cody -->

There are a few more configurable settings for your theme. You can switch the
bullet style between dots and arrows and you can choose whether or not to show
application borders by default. The *CSS* section allows you to enter custom CSS
that will also be served up by your theme. In this way, you can tweak a theme in
real time by adding new styles or overriding existing ones.

The next option configures the logo that appears for your site. 

### Using a Custom Logo for a Site

By default, the Liferay logo is used for your site pages' logo. If you want to
use your own logo for a specific site, use the *Logo* tab. Adding a custom logo
is easy: select the *Logo* tab from the *Edit Public Pages* interface and browse
to the location of your logo. Make sure your logo fits the space in the top left
corner of the theme you're using for your web site. If you don't, you could wind
up with a site that's difficult to navigate, as other page elements are pushed
aside to make way for the logo.

In the logo tab, you can also choose whether or not to display the site name on
the site. If you check the box labeled *Show Site Name*, the site name will
appear next to the logo. This option is enabled by default and cannot be
disabled if the *Allow Site Administrators to set their own logo* option is
disabled in *Instance Settings*. Removing the site name is not available for the
default site -- only newly created sites and user pages have the option to have
the name display. 

### Executing JavaScript in Site Pages

If you click on *JavaScript* for a page set (either Public Pages or Private
Pages), you'll find a window where you can enter JavaScript code that will be
executed at the bottom of every page in the site. If your site's theme uses
JavaScript (as is usually the case), it's best to add custom JavaScript code to
the theme and *not* in this window. This way, all of your site's JavaScript code
remains in one place.

Using the JavaScript window may be useful if your site's theme does *not* use
JavaScript. In this case, the JavaScript window will contain *all* of your
site's JavaScript and you can add some dynamic features to your site's pages.

Next, you'll look at an advanced feature of the *Edit Page* interface: merging
the current site's pages with the pages of the default site.

### Merging Pages From Other Sites

If you click on *Advanced* from the Edit Public Pages interface, you'll find an
option to merge the public pages of your instance's default site with the public
pages of the current site. If you enable this option, the pages of the default
site appear in the current site's navigation bar, along with the current site's
pages. Also, the pages of the current site appear in the navigation bar of the
default site, along with the default site's pages. This "merging" of pages only
affects the list of pages in the default site's and the current site's
*navigation bars*. This allows users to more easily navigate from the current
site to the default site, and vice versa. This option can be enabled for the
public pages of both personal sites and regular sites.

Note that this "merging" of pages is not a "hard merge". For example, suppose
that the site administrators of twenty different sites on your Liferay instance
all enabled the *Merge default site's public pages* option. Would the pages of
all these different sites be merged into each site's navigation bar? No, that
would make a mess! Instead, Liferay keeps track of the current `scopeGroupId`
(the ID of the current site) and the previous `scopeGroupId` (the ID of the
previously visited site). If the *Merge default site's public pages* option is
enabled for either the current site or the previous site, the pages of the
default site are merged in the pages of the other site.

For example, suppose that your Liferay instance has three sites: the default site, site A,
and site B. All three sites have some public pages. Site A has the *Merge
default site's public pages* option enabled; site B does not. When a user first
logs in, he's directed to the default site. The `scopeGroupId` is that of the
default site and there is no previous `scopeGroupId`, so no additional pages
appear in the default site's navigation bar. Then suppose the user navigates to
site A. Site A has the *Merge default site's public pages* option enabled, so
the default site's pages are added to site A's navigation bar. Now if the user
goes back to the default site, site A becomes the previous site so site A's
pages are added to the default site's navigation bar. If the user navigates to
site B, no additional pages appear in site B's navigation bar because site B
does not have the *Merge default site's public pages* option enabled. And if the
user navigates back to the default site, site B becomes the previous site, and,
again, since site B does not have the *Merge default site's public pages* option
enabled, no additional pages are added to the default site's navigation menu.

### Rendering Pages for Mobile Devices

You can configure your page set for mobile devices by selecting the *Mobile
Device Rules* option. The ability to modify themes for regular browsers and
mobile devices can only be accomplished by using this option. Mobile device
rules are inherited from your Public Pages, but you can define specific rules
per page. With the ability to define different rules per page, you can edit the
Look and Feel of specific pages for mobile devices, including the theme.

<!-- Add following sentence when this section is available (DXP only app) -Cody:
The set up for this option is explained in the
[Displaying Site Pages to Mobile Devices](/discover/portal/-/knowledge_base/7-0/displaying-site-pages-to-mobile-devices)
section.
-->

### Configuring Rules for Virtual Hosting

If you're using virtual hosting for this site, you can configure `robots.txt`
rules for the domain by selecting the *Robots* option. The Robots page gives you
the option to configure your `robots.txt` for both public and private pages on a
site. If you don't have Virtual Hosting set up, this tab is rather boring. 

### Notifying Search Engines of Site Pages

If you select the *Sitemap* option from the Site Pages interface for a page set,
you can send a sitemap to some search engines so they can crawl your site. It
uses the sitemap protocol, which is an industry standard. You can publish your
site to Yahoo or Google and their web crawlers will use the sitemap to index
your site. Liferay makes this very simple for administrators by
generating the sitemap XML for all public web sites.

By selecting one of the search engine links, the sitemap will be sent to them.
It's only necessary to do this once per site. The search engine crawler will
periodically crawl the sitemap once you've made the initial request.

If you're interested in seeing what is being sent to the search engines, select
the *preview* link to see the generated XML.

<!--
When configuring indiviual pages, you have several other options available. You
can categorize pages using tags and categories, which groups the page with other
similar assets such as web content, blogs, wikis, etc. This helps the page be
returned in search results. You also have the ability to include meta tags for
pages, which describes the contents of a page. You can also manage the embedded
applications on your page, according to the page template the page was built
from. Lastly, you have customization settings which let you set whether areas on
your page should be customizable by users with the appropriate permissions to do
so.
-->

Next, you'll learn how to customize individual site pages.

## Customizing Pages

When you decide to customize a single page, some different options that were not
available when initially creating a page appear. Customizing a specific page can
be done two ways; you can navigate to the site page and select the *Edit* button
from the bottom menu, or you can navigate the the Site Pages interface from the
Product Menu &rarr; *Sites* and select the specific page you'd like to edit from
the navigation tree. There are three groups that the options for customizing a
specific page can be defined under. You'll learn what each group offers for your
site pages.

### Managing Page Content

Managing your page's content drastically improves your page's organization and
user experience. The site page's configuration options offers some oppurtunities
to organize page content for your Lunar Resort site.

#### Categorization

You can explore ways to tag and categorize your page by clicking the
*Categorization* tab. These tools help administrators organize the page and
allows for users to easily find your page and its content through search and
navigation. For more information on using tags and categories, visit the
[Organizing Content with Tags and Categories](/discover/portal/-/knowledge_base/7-0/organizing-content-with-tags-and-categories)
section.

#### Custom Fields

*Custom Fields* lets you edit the custom fields you already have configured for
the *Page* resource. If you don't have any custom fields configured in your
site, this option is not available. If you don't have any custom fields
configured for the Page resource, you can navigate to the Control Panel &rarr;
*Custom Fields* located under the *Configuration* tab. These are metadata about
the page and can be anything you like, such as author or creation date. For more
information on Custom Fields, see the
[Custom Fields](/discover/portal/-/knowledge_base/6-2/custom-fields)
section.

<!-- TODO: replace link above. -Cody -->

### Improving Page Usability

One of the most important tasks for administrators is ensuring the user
experience is easy and enjoyable. There are configuration options that aid in
providing a seamless user experience for your site page.

#### Details

The first option you're given (and the default option selected when customizing
an indiviual page) is *Details*. This option lets you name the page for any
localizations you need, set whether the page is hidden on the navigation menu,
set an easy to remember, friendly URL for the page, and select the page type.
Plus you can specify how apps are arranged on a page. Choose from the
available installed templates to modify the layout. It's very easy for
developers to define custom layouts and add them to the list. This is covered
more thoroughly in the
[Creating a Layout Template Project in the Plugins SDK](/develop/tutorials/-/knowledge_base/6-2/creating-a-layout-template-project-in-the-plugins-sdk)
tutorial.

<!-- TODO: Update tutorial link to 7.0, when available. -Cody -->

#### Look and Feel

The *Look and Feel* option lets you set a page-specific theme. You can inherit
what you already have configured for your page sets' theme, or you can uniquely
define them per page. You can see the Page Set's
[Customizing the Look and Feel of Site Pages](/discover/portal/-/knowledge_base/7-0/creating-and-managing-pages#customizing-the-look-and-feel-of-site-pages)
section for more details.

#### Mobile Device Rules

This option allows you to apply rules for how this page should be rendered for
various mobile devices. You can set these up in the *Mobile Device Rules*
section in the Product Menu &rarr; *Sites* &rarr; *Configuration*. For more
information about Mobile Device Rules and how to use them, see the
[Displaying Site Pages to Mobile Devices]()
section.

<!-- TODO: replace link above. -Cody -->

#### Embedded Portlets

This option only appears if you have embedded one or more portlets on
the page. Previous to Liferay 7.0, you were able to embed portlets on a page by
acquring their portlet ID and using a `runtime-portlet` tag in the Web Content
Display app to embed a portlet on a page. This is no longer possible.

Applications can now be embedded on a page via web content template. To learn
more about this, see the [Adding Templates](/discover/portal/-/knowledge_base/7-0/designing-uniform-content#adding-templates)
section. Also, you can embed a portlet on a page layout or theme programtically.
If you're interested in learning more about this, visit the
[Embedding Portlets in Themes and Layout Templates](/develop/tutorials/-/knowledge_base/7-0/embedding-portlets-in-themes-and-layout-templates)
tutorial.

#### Customization Settings

This configuration option lets you mark specific sections of the page you want
users to be able to customize. You can learn more about page customizations in
the [Personalizing Pages]()
section.

<!-- TODO: replace link above. -Cody -->

### Enhancing Page Intelligence

Your page's configuration options offers several oppurtunities to improve your
page's data optimization and JavaScript, among others. To enhance your page's
intelligence, configure the options described below.

#### SEO

*SEO* provides several means of optimizing the data the page provides to an
indexer that's crawling the page. You can set the various meta tags for
description, keywords and robots. There's also a separate Robots section that
lets you tell indexing robots how frequently the page is updated and how it
should be prioritized. If the page is localized, you can select a box to make
Liferay generate canonical links by language. If you want to set some of these
settings for the entire site, you can specify them from the Sitemaps and Robots
tabs of the Manage Site Settings dialog box (see below).

In previous versions of Liferay, it was possible that a single page
could be indexed multiple times. In Liferay 6.1, all URLs that direct to the
same page will only create one entry in the index. Previously, the simple URL
*http\://www.lunar-resort.com/web/guest/blog/-/blogs/themoon* and different
versions of the URL which provided additional information about the referring
page had different entries in the index. In Liferay 7.0, each asset (web
content article, blog entry, etc.) has a unique URL. From the search engine's
point of view, this will make your pages rank higher since any references to
variations of a specific URL will all be considered references to the same
page.] 

#### JavaScript

If you click on *JavaScript*, you'll find a window where you can enter
JavaScript code that will be executed at the bottom of your page.
If your site's theme uses JavaScript (as is usually the case), it's best to add
custom JavaScript code to the theme and *not* in this window. This way, all of
your site's JavaScript code remains in one place.

This configuration option is also available for page sets like Public Pages and
Private Pages. Visit the
[Executing JavaScript in Site Pages](/discover/portal/-/knowledge_base/7-0/creating-and-managing-pages#executing-javascript-in-site-pages)
section for more information on doing this for page sets.

#### Advanced

The *Advanced* option contains several optional features. You can set a query
string to provide parameters to the page. This can become useful to web content
templates, which you'll see in the next chapter. You can set a target for the
page so that it either pops up in a particularly named window or appears in a
frameset. And you can set an icon for the page that appears in the navigation
menu.

Next, you'll practice personalizing pages using page customizations!

## Adding Portlets to a Page

As we discussed earlier, Liferay pages are composed of applications. All of your
site's functionality, from blogs to shopping, is composed of apps. Even static
web content can be displayed through Web Content Display apps. To add an app to
a page, just click the *Add* button
(![Add](../../../images/icon-control-menu-add.png)) from the top menu and select
the *Applications* tab. You can either browse through the categories of
available apps until you find the one you're looking for or you can search for
apps by name. Once you've found an app, click the *Add* button to add it to the
current page. Once it's been added to the page, you can drag it to a new
position. Alternatively, you can drag the app directly from the Applications
menu to a specific location on the page. Follow the steps below to add some
Collaboration apps to the Lunar Resort site.

1. From the top menu, select *Add* &rarr; *Applications*.
2. In the menu that appears, expand the *Collaboration* category.
3. Drag the *Blogs Aggregator* app from the Add Application window to the right
   column of your page.
4. Next, drag the *Wiki* app to the left column.

See how easy it is to add applications to your pages? You've added the Wiki
app and Blogs Aggregator app to a page. 

![Figure 6: Your page layout options are virtually limitless with a slew of application and layout combinations.](../../../images/app-layout-design.png)

It's easy to make your pages look exactly the way you want them to. If the
default layout options provided aren't enough, you can even develop your own.
To find more information about developing custom layout templates, please refer
to the [Creating a Layout Template Project in the Plugins SDK](/develop/tutorials/-/knowledge_base/6-2/creating-a-layout-template-project-in-the-plugins-sdk)
tutorial. 

<!-- TODO: Replace link above. -Cody -->

Next, you'll practice personalizing pages using page customizations!

## Personalizing Pages

Administrators can designate public pages or sections of public pages to be
customizable. When a user visits such a page, a notification appears stating
that the user can customize the page. Users can make customizations only in the
sections of pages designated by administrators. Customizations are based on the
rows and columns of a page layout. Page customizations are only visible to the
user who made the customizations. By default, site members can make page
customizations but non-site members and guests can't.

To enable page customizations as an administrator, first navigate to the page
you'd like to let site members modify. Then click on the *Edit* button at the
left side of the page, expand the *Customization Settings* area, and click on
the *Show Customizable Sections* button. 

<!-- Not available at the current moment in 7.0. -Cody -->

![Figure 8.1: To enable page customizations, click on the *Edit* button at the left side of the page, expand the *Customization Settings* area, and click on the *Show Customizable Sections* button.](../../../images/page-customizations.png)

Once you've clicked the *Show Customizable Sections* button, you'll see one or
more red regions, depending on the layout template of your page. Check one or
more of the *Customizable* boxes to allow site members to customize certain
sections of the page. Regions that you've designated as customizable are colored
green.

![Figure 8.2: Check one or more of the *Customizable* boxes to allow site members to customize certain sections of the page.](../../../images/customizable-regions.png)

When site members visit your customizable page, they'll see a notification
saying, "You can customize this page." Site members can toggle between viewing
their customized page and viewing the default page by clicking the *View Default
Page* or *View My Customized Page* links just below the Dockbar. There's also a
*Reset My Customizations* link that restores a user's customized page to the
match the default page. This allows users to discard one set of customizations
and start a new set without having to manually undo each customization that
they'd previously made.

Note that non-administrator site members can access the Add menu from the left
side of the screen when viewing their customizable page even if they don't
ordinarily have permission to view this menu. This allows them to add portlets
to the sections of the page that they're allowed to customize. If they click
*View Default Page*, the Add menu will disappear from the Dockbar since they're
not allowed to modify the default page.

![Figure 8.3: Non-administrator site members can customize their own versions of customizable pages but can't modify the default page.](../../../images/default-customizable-page.png)

Administrators of customizable pages have the same two views as site members:
the *default page* view and the *customized page* view. Changes made to the
*default page* affect all users, whereas changes made to the *customized page*
affect only the administrator who made the changes. Changes made by
administrators to non-customizable sections in the *default view* are
immediately applied for all users. However, changes made by administrators to
customizable sections do *not* overwrite users' customizations.

Users can make two kinds of customizations to customizable regions. First, they
can configure portlet applications within the customizable regions. Second, they
can add portlets to or remove portlets from the customizable regions.

The portal doesn't allow users to change a non-instanceable portlet's
configuration inside a customizable region since those kinds of portlets are
tied to the site to which they've been added. If this were allowed, the
customization would affect all users, not just the one who customized the
region. Therefore, changes to the portlet configuration in a customizable region
are only possible for instanceable portlets, whose portlet configuration only
affects that one user.

For example, suppose that you, as an administrator, selected the right column of
the Welcome page of the Lunar Resort site to be customizable. A member of the
Lunar Resort site could take the following steps to make a personal
customization of the Welcome page:

1. Navigate to the Welcome homepage by clicking *Go To* &rarr; *Lunar Resort*
   from the Dockbar.
2. Remove the Hello World portlet from the right column of the page.
3. Add the Language portlet to the right column of the page by clicking *Add*,
   clicking on *Applications*, searching for *Language*, and clicking *Add* next
   to its name.
4. Configure the Language portlet by clicking on the gear icon and selecting
   *Configuration* and then opening the *Display Style* dropdown menu and
   choosing *Select Box*.

The Language portlet is useful to have on your portal's homepage if you expect
users who speak different languages to access your portal. Users can select
their language in the Language portlet to view a translation of the portal into
their native language. After closing the Configuration dialog box of the
Language portlet, the customized Welcome page looks like this:

![Figure 8.4: In this example, the user removed the Hello World portlet, added the Language portlet, and changed the display style from icons to a select box.](../../../images/customized-portal-homepage.png)

<!-- This section needed updating for 6.2. The following two paragraphs'
statements about the required permissions and roles seem to be obsolete. -->

To allow users to customize a page, administrators must grant users permission
to *Customize* pages under the Site section. This can be achieved by assigning
permission to a role, then assigning this role to the appropriate users. For
example, if we want any logged user to be able to customize our customizable
pages, we could assign the *Customize* permission to the role *User*. If we want
site members to be able to customize the customizable pages of their sites, we
would accept the default setting. By default, the *Customize* permission is
assigned to the role *Site Member*.

In addition to granting the ability to customize portlet configurations, the
*Customize* permission allows users to customize the look and feel of portlets
and to import or export portlet settings. Next, let's look at how to use
application display templates.

## Changing Page Permissions

By default, public pages are just that: public. They can be viewed by anybody,
logged in or not logged in. And private pages are really only private from
non-members of the site. If someone has joined your site or is a member of your
organization, that person can see all the private pages. You can, however,
modify the permissions on individual pages in either page group so only certain
users can view them.

Suppose you want to create a page only for administrators to see. You can do
this with the following procedure:

1. Go to the Dockbar and select *Admin* &rarr; *Site Administration*.
2. Click the *Site Pages* link (if necessary).
3. Click the *Private Pages* tab to switch to the Private Pages. Remember,
   these pages by default are viewable only by members of the site.
4. Create a page called *Admin Tips*.
5. Click on the page in the tree on the left and then click *Permissions*.
6. Uncheck the *View* and *Add Discussion* permissions next to the Site Member
   role.
7. Click the *Save* button.

![Figure 2.13: The Permissions offer a plethora of options for each role.](../../../images/04-web-content-page-permissions.png)

Congratulations! You've changed the permissions for this page so only site
administrators can view it. Any users you add to this role can now see the page.
Other users, even members of this site, won't have permission to see it.

Pages in Liferay are as flexible as pages you'd create manually without a
portal. Using a point and click interface, you can define your site any way you
want. You can create and remove pages, export and import them, set their
layouts, define how they are indexed by search engines and more.

You now understand how to manage pages in Liferay Portal. It's time to move on
to adding content to those pages. Liferay's Web Content Management (WCM) is a
highly powerful, yet flexible, set of tools that enables you to successfully
manage your web site.

You'll soon discover that Liferay's WCM is easy to learn and highly
configurable. If you already have experience with WCM, you'll see some new
features and improvements to old ones. If you're new to Liferay's WCM, then
you'll be surprised at how fast you will be adding, editing and scheduling
content on your site. Once you're familiar with portlets such as Web Content
Display and Asset Publisher, your ability to manage an immense site with a large
amount of content will simply amaze you.

You'll be using Liferay's Web Content Management system to publish simple pieces
of content, develop templates to define how content is to be displayed, set up a
workflow for content to be approved, schedule when content is to be published
and much, much more. You'll learn about many of these features in the next
section.

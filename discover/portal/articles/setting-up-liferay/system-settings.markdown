# System Settings

You might be tired of hearing about it by now, but it's worth repeating: Liferay
is modular. It's composed of many applications. Those applications are
configurable at several scopes. If you need to make configuration changes to an
application that take effect in a system-wide fashion, where do you do it?
Readers who have been around a while will be raising their hands hoping to be
called on, sure that the correct answer is "why, system-wide application
configuration takes place in a properties file, most commonly the
`portal-ext.properties` file". They then expect to get a sticker of [Ray](https://www.liferay.com/ray) on their mobile device for their attentiveness. However, that's an
incomplete answer, so they need to just sit back down and listen like the rest
of the class. There's a brand new way to make configuration changes at the
system scope in Liferay 7, and you don't need to go messing around in a
properties file to do it. These settings can now be made in the Control Panel,
in *Configuration* &rarr; *System Settings*. 

![Figure 1: System Settings are accessed through the Control Panel.](../../images/system-settings-product-menu.png)

In The Lunar Resort, there's a forum site for excited guests to discuss their
visits and offer guidance and tips to new guests. Sometimes they get a little
rowdy, and need to be temporarily banned from the site's forum (Liferay's forum
app is the Message Boards portlet). The default ban interval is ten days. If that
seems a little extreme to you, The Lunar Resort's administrator, then change it. 

## Editing The Default Configuration

Once you navigate to System Settings, you'll see that the configuration options
are categorized into logical groupings. 

![Figure 2: System Settings are organized by component.](../../images/system-settings-components.png)

If you know that you want to change the default configuration of the Message
Boards application, you can probably deduce that its configuration entries will
be in the Collaboration section. Once you click on that heading, you'll only see
the configuration entries for that category, listed in alphabetical order (so
Blogs and the related Blogs Service entries are right at the top of the list).
If you don't see the configuration entry you need, check in the Other category.
Browse the remaining categories as a last resort.

Once you find the entry you're looking for, click its Actions button
(![Actions](../../images/icon-actions.png)), then click *Edit*.

You're taken to a simple interface for changing whatever default configuration
options you want.

<!--Mention search? Or a link to the application categories? They're not the same
categories as exist in the Add Applications menu-->




## Configuration Scope

While browsing the categories of System Settings, you'll notice that each entry
has a Name and a Scope. The Name is pretty self-explanatory, but the Scope is
worth discussing. There are at least four values that you'll see under Scope:

System

Default Configuration for Application

Default Configuration for All Sites

![Figure X: Each System Settings entry will have a configuration Scope.](../../images/system-settings-scope.png)

<!-- Need a Lunar Resort example -->

<!-- Configuration Scopes diagram should go here, too -->

<!-- Make sure to note that more granular configurations override system-wide
ones, and that UI configurations override those set in properties files. -->

<!-- Cover Reset Default Values option -->










![Figure 4: System Settings are accessed through the Control Panel.](../../images/system-settings-product-menu.png)





Default Settings for All Sites

- Appears to be overridden from Site Admin. For example, see the Blogs Service
configuration entry, which has the scope of *Default Settings For All Sites*. So
when you change something here, it affects the default configuration options in
Content &rarr; Blogs &rarr; Configuration.

<!--![Figure X: IMAGE OF SIDE_BY_SIDE CONFIGURATION IN SITE ADMIN AND CONFIG
ADMIN]()-->

System

Override system-wide stuff, like FreeMarker engine configuration, CMIS Store,
Elasticsearch, and more.

Default Configuration for application

- Override in the portlet instance, once deployed on a page. Click the options
![Options](../../images/icon-options.png) menu then Configuration to override
these settings 

Default Configuration For All Instances

See LDAP configuration entries in Platform

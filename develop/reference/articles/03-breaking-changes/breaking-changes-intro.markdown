# What are the Breaking Changes for Liferay 7.0? [](id=what-are-the-breaking-changes-for-liferay-7-0)

This document presents a chronological list of changes that break existing
functionality, APIs, or contracts with third party Liferay developers or users.
We try our best to minimize these disruptions, but sometimes they are
unavoidable.

Here are some of the types of changes documented in this file:

* Functionality that is removed or replaced
* API incompatibilities: Changes to public Java or JavaScript APIs
* Changes to context variables available to templates
* Changes in CSS classes available to Liferay themes and portlets
* Configuration changes: Changes in configuration files, like
 `portal.properties`, `system.properties`, etc.
* Execution requirements: Java version, J2EE Version, browser versions, etc.
* Deprecations or end of support: For example, warning that a certain
feature or API will be dropped in an upcoming version.
* Recommendations: For example, recommending using a newly introduced API that
replaces an old API, in spite of the old API being kept in Liferay Portal for
backwards compatibility.

## Breaking Changes List [](id=breaking-changes-list)

### The liferay-ui:logo-selector Tag Requires Parameter Changes [](id=the-liferay-uilogo-selector-tag-requires-parameter-changes)
- **Date:** 2013-Dec-05
- **JIRA Ticket:** LPS-42645

#### What changed? [](id=what-changed)

The Logo Selector tag now supports uploading an image, storing it as a temporary
file, cropping it, and canceling edits. The tag no longer requires creating a UI
to include the image. Consequently, the `editLogoURL` parameter is no longer
needed and has been removed. The tag now uses the following parameters to
support the new features:

- `currentLogoURL`: the URL to display the image being stored
- `hasUpdateLogoPermission`: `true` if the current user can update the logo
- `maxFileSize`: the size limit for the logo to be uploaded
- `tempImageFileName`: the unique identifier to store the temporary image on
upload

#### Who is affected? [](id=who-is-affected)

Plugins or templates that are using the `liferay-ui:logo-selector` tag need
to update their usage of the tag.

#### How should I update my code? [](id=how-should-i-update-my-code)

You should remove the parameter `editLogoURL` and include (if neccessary) the
parameters `currentLogoURL`, `hasUpdateLogoPermission`, `maxFileSize`, and/or
`tempImageFileName`.

**Example**

Old way:

    <portlet:renderURL var="editUserPortraitURL" windowState="<%= LiferayWindowState.POP_UP.toString() %>">
        <portlet:param name="struts_action" value="/users_admin/edit_user_portrait" />
        <portlet:param name="redirect" value="<%= currentURL %>" />
        <portlet:param name="p_u_i_d" value="<%= String.valueOf(selUser.getUserId()) %>" />
        <portlet:param name="portrait_id" value="<%= String.valueOf(selUser.getPortraitId()) %>" />
    </portlet:renderURL>

    <liferay-ui:logo-selector
        defaultLogoURL="<%= UserConstants.getPortraitURL(themeDisplay.getPathImage(), selUser.isMale(), 0) %>"
        editLogoURL="<%= editUserPortraitURL %>"
        imageId="<%= selUser.getPortraitId() %>"
        logoDisplaySelector=".user-logo"
    />

New way:

    <liferay-ui:logo-selector
        currentLogoURL="<%= selUser.getPortraitURL(themeDisplay) %>"
        defaultLogoURL="<%= UserConstants.getPortraitURL(themeDisplay.getPathImage(), selUser.isMale(), 0) %>"
        hasUpdateLogoPermission='<%= UsersAdminUtil.hasUpdateFieldPermission(selUser, "portrait") %>'
        imageId="<%= selUser.getPortraitId() %>"
        logoDisplaySelector=".user-logo"
        maxFileSize="<%= PrefsPropsUtil.getLong(PropsKeys.USERS_IMAGE_MAX_SIZE) / 1024 %>"
        tempImageFileName="<%= String.valueOf(selUser.getUserId()) %>"
    />

#### Why was this change made? [](id=why-was-this-change-made)

This change helps keep a unified UI and consistent experience for uploading
logos in the portal. The logos can be customized from a single location and used
throughout the portal. In addition, the change adds new features such as image
cropping and support for canceling image upload.

---------------------------------------

### Merged Configured Email Signature Field into the Body of Email Messages from Message Boards and Wiki [](id=merged-configured-email-signature-field-into-the-body-of-email-messages-fro)
- **Date:** 2014-Feb-28
- **JIRA Ticket:** LPS-44599

#### What changed? [](id=what-changed-0)

The configuration for email signatures of notifications from Message Boards and
Wiki has been removed. An automatic update process is available that appends
existing signatures into respective email message bodies for Message Boards and
Wiki notifications. The upgrade process only applies to configured signatures in
the database. In case you declared signatures in portal properties (e.g.,
`portal-ext.properties`), you must make the manual changes explained below.

#### Who is affected? [](id=who-is-affected-0)

Users and system administrators who have configured email signatures for Message
Boards or Wiki notifications are affected. System administrators who have
configured portal properties (e.g., `portal-ext.properties`) must make the
manual changes described below.

#### How should I update my code? [](id=how-should-i-update-my-code-0)

You should modify your `portal-ext.properties` file to remove the properties
`message.boards.email.message.added.signature`,
`message.boards.email.message.updated.signature`,
`wiki.email.page.added.signature`, and `wiki.email.page.updated.signature`.
Then, you should append the contents of the signatures to the bodies you had
previously configured in your `portal-ext.properties` file.

**Example**

Old way:

    wiki.email.page.updated.body=A wiki page was updated.
    wiki.email.page.updated.signature=For any doubts email the system administrator

New way:

    wiki.email.page.updated.body=A wiki page was updated.\n--\nFor any doubts email the system administrator

#### Why was this change made? [](id=why-was-this-change-made-0)

This change helps simplify the user interface. The signatures can still be set
inside the message body. There was no real benefit in keeping the signature and
body fields separate.

---------------------------------------

### Removed get and format Methods that Used PortletConfig Parameters [](id=removed-get-and-format-methods-that-used-portletconfig-parameters)
- **Date:** 2014-Mar-07
- **JIRA Ticket:** LPS-44342

#### What changed? [](id=what-changed-1)

All the methods `get()` and `format()` which had the PortletConfig as a
parameter have been removed.

#### Who is affected? [](id=who-is-affected-1)

Any invocations from Java classes or JSPs to these methods in `LanguageUtil` and
`UnicodeLanguageUtil` are affected.

#### How should I update my code? [](id=how-should-i-update-my-code-1)

Replace invocations to these methods with invocations to methods of the same
name that take a `ResourceBundle` parameter, instead of taking a
`PortletConfig` parameter.

**Example**

Old call:

    LanguageUtil.get(portletConfig, locale, key);

New call:

    LanguageUtil.get(portletConfig.getResourceBundle(locale), key);

#### Why was this change made? [](id=why-was-this-change-made-1)

The removed methods didn't work properly and would never work properly, since
they didn't have all the information they required. Since we expected the
methods were rarely used, we thought it better to remove them without
deprecation than to leave them as buggy methods in the API.

---------------------------------------

### Web Content Articles Now Require a Structure and Template [](id=web-content-articles-now-require-a-structure-and-template)
- **Date:** 2014-Mar-18
- **JIRA Ticket:** LPS-45107

#### What changed? [](id=what-changed-2)

Web content is now required to use a structure and template. A default structure
and template named *Basic Web Content* was added to the global scope, and can be
modified or deleted.

#### Who is affected? [](id=who-is-affected-2)

Applications that use the Journal API to create web content without a structure
or template are affected.

#### How should I update my code? [](id=how-should-i-update-my-code-2)

You should always use a structure and template when creating web content. You
can still use the *Basic Web Content* from the global scope (using the
structure key `basic-web-content`), but you should keep in mind that users can
modify or delete it.

#### Why was this change made? [](id=why-was-this-change-made-2)

This change gives users the flexibility to modify the default structure and
template.

---------------------------------------

### Changed the AssetRenderer and Indexer APIs to Include the PortletRequest and PortletResponse Parameters [](id=changed-the-assetrenderer-and-indexer-apis-to-include-the-portletrequest-an)
- **Date:** 2014-May-07
- **JIRA Ticket:** LPS-44639 and LPS-44894

#### What changed? [](id=what-changed-3)

The `getSummary()` method in the AssetRenderer API and the `doGetSummary()`
method in the Indexer API have changed and must include a `PortletRequest`
and `PortletResponse` parameter as part of their signatures.

#### Who is affected? [](id=who-is-affected-3)

These methods must be updated in all AssetRenderer and Indexer implementations.

#### How should I update my code? [](id=how-should-i-update-my-code-3)

Add a `PortletRequest` and `PortletResponse` parameter to the signatures of
these methods.

**Example 1**

Old signature:

    protected Summary doGetSummary(Document document, Locale locale, String snippet, PortletURL portletURL)

New signature:

    protected Summary doGetSummary(Document document, Locale locale, String snippet, PortletRequest portletRequest, PortletResponse portletResponse)

**Example 2**

Old signature:

    public String getSummary(Locale locale)

New signature:

    public String getSummary(PortletRequest portletRequest, PortletResponse portletResponse)

#### Why was this change made? [](id=why-was-this-change-made-3)

Some content (such as web content) needs the `PortletRequest` and
`PortletResponse` parameters in order to be rendered.

---------------------------------------

### Only One Portlet Instance's Settings is Used Per Portlet [](id=only-one-portlet-instances-settings-is-used-per-portlet)
- **Date:** 2014-Jun-06
- **JIRA Ticket:** LPS-43134

#### What changed? [](id=what-changed-4)

Previously, some portlets allowed separate setups per portlet instance,
regardless of whether the instances were in the same page or in different pages.
For some of the portlet setup fields, however, it didn't make sense to allow
different values in different instances. The flexibility of these fields was
unnecessary and confused users. As part of this change, these fields have been
moved from portlet instance setup to Site Administration.

The upgrade process takes care of making the necessary database changes. In the
case of several portlet instances having different configurations, however, only
one configuration is preserved.

For example, if you configured three Bookmarks portlets where the mail
configuration was the same, upgrade will be the same and you won't have any
problem. But if you configured the three portlet instances differently, only one
configuration will be chosen. To find out which configuration is chosen, you can
check the log generated in the console by the upgrade process.

Since configuring instances of the same portlet type differently is highly
discouraged and notoriously problematic, we expect this change will
inconvenience only a very low minority of portal users.

#### Who is affected? [](id=who-is-affected-4)

Affected users are those who have specified varying configurations for multiple
portlet instances of a portlet type, that stores configurations at the layout
level.

#### How should I update my code? [](id=how-should-i-update-my-code-4)

The upgrade process chooses one portlet instance's configurations and stores it
at the service level. After the upgrade, you should review the portlet's
configuration and make any necessary modifications.

#### Why was this change made? [](id=why-was-this-change-made-4)

Unifying portlet and service configuration facilitates managing them.

---------------------------------------

### DDM Structure Local Service API No Longer Has the updateXSDFieldMetadata operation [](id=ddm-structure-local-service-api-no-longer-has-the-updatexsdfieldmetadata-op)
- **Date:** 2014-Jun-11
- **JIRA Ticket:** LPS-47559

#### What changed? [](id=what-changed-5)

The `updateXSDFieldMetadata()` operation was removed from the DDM Structure
Local Service API.

DDM Structure Local API users should reference a structure's internal
representation; any call to modify a DDM structure's content should be done
through the DDMForm model.

#### Who is affected? [](id=who-is-affected-5)

Applications that use the DDM Structure Local Service API might be affected.

#### How should I update my code? [](id=how-should-i-update-my-code-5)

You should always use DDMForm to update the DDM Structure content. You can
retrieve it by calling `ddmStructure.getDDMForm()`. Perform any changes to it and
then call `DDMStructureLocalServiceUtil.updateDDMStructure(ddmStructure)`.

#### Why was this change made? [](id=why-was-this-change-made-5)

This change gives users the flexibility to modify the structure content without
concerning themselves with the DDM Structure's internal content representation
of data.

---------------------------------------

### The aui:input Tag for Type checkbox No Longer Creates a Hidden Input [](id=the-auiinput-tag-for-type-checkbox-no-longer-creates-a-hidden-input)
- **Date:** 2014-Jun-16
- **JIRA Ticket:** LPS-44228

#### What changed? [](id=what-changed-6)

Whenever the aui:input tag is used to generate an input of type checkbox,
only an input tag will be generated, instead of the checkbox and hidden field it
was generating before.

#### Who is affected? [](id=who-is-affected-6)

Anyone trying to grab the previously generated fields is affected. The change
mostly affects JavaScript code trying to add some additional actions when
clicking on the checkboxes.

#### How should I update my code? [](id=how-should-i-update-my-code-6)

In your front-end JavaScript code, follow these steps:

- Remove the `Checkbox` suffix when querying for the node in any of its forms,
like `A.one(...)`, `$(...)`, etc.
- Remove any action that tries to set the value of the checkbox on the
previously generated hidden field.

#### Why was this change made? [](id=why-was-this-change-made-6)

This change makes generated forms more standard and interoperable since it falls
back to the checkboxes default behavior. It allows the form to be submitted
properly even when JavaScript is disabled.

---------------------------------------

### Using util-taglib No Longer Binds You to Using portal-service's javax.servlet.jsp Implementation [](id=using-util-taglib-no-longer-binds-you-to-using-portal-services-javax-servle)
- **Date:** 2014-Jun-19
- **JIRA Ticket:** LPS-47682

#### What changed? [](id=what-changed-7)

Several APIs in `portal-service.jar` contained references to the
`javax.servlet.jsp` package. This forced `util-taglib`, which depended on many
of the package's features, to be bound to the same JSP implementation.

Due to this, the following APIs had breaking changes:

- `LanguageUtil`
- `UnicodeLanguageUtil`
- `VelocityTaglibImpl`
- `ThemeUtil`
- `RuntimePageUtil`
- `PortletDisplayTemplateUtil`
- `DDMXSDUtil`
- `PortletResourceBundles`
- `ResourceActionsUtil`
- `PortalUtil`

#### Who is affected? [](id=who-is-affected-7)

This affects anyone calling the classes listed above.

#### How should I update my code? [](id=how-should-i-update-my-code-7)

Code invoking the APIs listed above should be updated to use an
`HttpServletRequest` parameter instead of the formerly used `PageContext`
parameter.

#### Why was this change made? [](id=why-was-this-change-made-7)

As stated previously, the use of the `javax.servlet.jsp` API in `portal-service`
prevented the use of any other JSP impl within plugins (OSGi or otherwise). This
limited what Liferay could change with respect to providing its own JSP
implementation within OSGi.

---------------------------------------

### Changes in Exceptions Thrown by User Services [](id=changes-in-exceptions-thrown-by-user-services)
- **Date:** 2014-Jul-03
- **JIRA Ticket:** LPS-47130

#### What changed? [](id=what-changed-8)

In order to provide more information about the root cause of an exception,
several exceptions have been extended with static inner classes, one for each
cause. As a result of this effort, some exceptions have been identified that
really belong as static inner subclasses of existing exceptions.

#### Who is affected? [](id=who-is-affected-8)

Client code which is handling any of the following exceptions:

- `DuplicateUserScreenNameException`
- `DuplicateUserEmailAddressException`

#### How should I update my code? [](id=how-should-i-update-my-code-8)

Replace the old exception with the equivalent inner class exception as follows:

- `DuplicateUserScreenNameException` &rarr;
`UserScreenNameException.MustNotBeDuplicate`
- `DuplicateUserEmailAddressException` &rarr;
`UserEmailAddressException.MustNotBeDuplicate`

#### Why was this change made? [](id=why-was-this-change-made-8)

This change provides more information to clients of the services API about the
root cause of an error. It provides a more helpful error message to the end-user
and it allows for easier recovery, when possible.

---------------------------------------

### Removed Trash Logic from DLAppHelperLocalService Methods [](id=removed-trash-logic-from-dlapphelperlocalservice-methods)
- **Date:** 2014-Jul-22
- **JIRA Ticket:** LPS-47508

#### What changed? [](id=what-changed-9)

The `deleteFileEntry()` and `deleteFolder()` methods in
`DLAppHelperLocalService` deleted the corresponding trash entry in the database.
This logic has been removed from these methods.

#### Who is affected? [](id=who-is-affected-9)

Every caller of the `deleteFileEntry()` and `deleteFolder()` methods is
affected.

#### How should I update my code? [](id=how-should-i-update-my-code-9)

There is no direct replacement. Trash operations are now accessible through the
`TrashCapability` implementations for each repository. The following code
demonstrates using a `TrashCapability` instance to delete a `FileEntry`:

    Repository repository = getRepository();

    TrashCapability trashCapability = repository.getCapability(
        TrashCapability.class);

    FileEntry fileEntry = repository.getFileEntry(fileEntryId);

    trashCapability.deleteFileEntry(fileEntry);

Note that the `deleteFileEntry()` and `deleteFolder()` methods in
`TrashCapability` not only remove the trash entry, but also remove the folder or
file entry itself, and any associated data, such as assets, previews, etc.

#### Why was this change made? [](id=why-was-this-change-made-9)

This change was made to allow different kinds of repositories to support trash
operations in a uniform way.

---------------------------------------

### Removed Sync Logic from DLAppHelperLocalService Methods [](id=removed-sync-logic-from-dlapphelperlocalservice-methods)
- **Date:** 2014-Sep-05
- **JIRA Ticket:** LPS-48895

#### What changed? [](id=what-changed-10)

The `moveFileEntry()` and `moveFolder()` methods in `DLAppHelperLocalService`
fired Liferay Sync events. These methods have been removed.

#### Who is affected? [](id=who-is-affected-10)

Every caller of the `moveFileEntry()` and `moveFolder()` methods is affected.

#### How should I update my code? [](id=how-should-i-update-my-code-10)

There is no direct replacement. Sync operations are now accessible through the
`SyncCapability` implementations for each repository. The following code
demonstrates using a `SyncCapability` instance to move a `FileEntry`:

    Repository repository = getRepository();

    SyncCapability syncCapability = repository.getCapability(
        SyncCapability.class);

    FileEntry fileEntry = repository.getFileEntry(fileEntryId);

    syncCapability.moveFileEntry(fileEntry);

#### Why was this change made? [](id=why-was-this-change-made-10)

There are repositories that don't support Liferay Sync operations.

---------------------------------------

### Removed the .aui Namespace from Bootstrap [](id=removed-the-aui-namespace-from-bootstrap)
- **Date:** 2014-Sep-26
- **JIRA Ticket:** LPS-50348

#### What changed? [](id=what-changed-11)

The `.aui` namespace was removed from prefixing all of Bootstrap's CSS.

#### Who is affected? [](id=who-is-affected-11)

Theme and plugin developers that targeted their CSS to rely on the namespace are
affected.

#### How should I update my code? [](id=how-should-i-update-my-code-11)

Theme developers can still manually add an `aui.css` file in their `_diffs`
directory, and add it back in. The `aui` CSS class can also be added to the
`$root_css_class` variable.

#### Why was this change made? [](id=why-was-this-change-made-11)

Due to changes in the Sass parser, the nesting of third-party libraries was
causing some syntax errors which broke other functionality (e.g., RTL
conversion). There was also a lot of additional complexity for a relatively
minor benefit.

---------------------------------------

### Moved MVCPortlet, ActionCommand and ActionCommandCache from util-bridges.jar to portal-service.jar [](id=moved-mvcportlet-actioncommand-and-actioncommandcache-from-util-bridges-jar)
- **Date:** 2014-Sep-26
- **JIRA Ticket:** LPS-50156

#### What changed? [](id=what-changed-12)

The classes from package `com.liferay.util.bridges.mvc` in `util-bridges.jar`
were moved to a new package `com.liferay.portal.kernel.portlet.bridges.mvc`
in `portal-service.jar`.

Old classes:

    com.liferay.util.bridges.mvc.ActionCommand
    com.liferay.util.bridges.mvc.BaseActionCommand

New classes:

    com.liferay.portal.kernel.portlet.bridges.mvc.BaseMVCActionCommand
    com.liferay.portal.kernel.portlet.bridges.mvc.MVCActionCommand

In addition, `com.liferay.util.bridges.mvc.MVCPortlet` is deprecated, but was
made to extend `com.liferay.portal.kernel.portlet.bridges.mvc.MVCPortlet`.

The classes in the `com.liferay.portal.kernel.portlet.bridges.mvc` package have
been renamed to add the `MVC` prefix. These modifications were made after this
breaking change, and can be referenced in
[LPS-56372](https://issues.liferay.com/browse/LPS-56372).

#### Who is affected? [](id=who-is-affected-12)

This will affect any implementations of `ActionCommand`.

#### How should I update my code? [](id=how-should-i-update-my-code-12)

Replace imports of `com.liferay.util.bridges.mvc.ActionCommand` with
`com.liferay.portal.kernel.portlet.bridges.mvc.ActionCommand` and imports of
`com.liferay.util.bridges.mvc.BaseActionCommand` with
`com.liferay.portal.kernel.portlet.bridges.mvc.BaseActionCommand`.

#### Why was this change made? [](id=why-was-this-change-made-12)

This change was made to avoid duplication of an implementable interface in the
system. Duplication can cause `ClassCastException`s.

---------------------------------------

### Convert Process Classes Are No Longer Specified via the convert.processes Portal Property, but Are Contributed as OSGi Modules [](id=convert-process-classes-are-no-longer-specified-via-the-convert-processes-p)
- **Date:** 2014-Oct-09
- **JIRA Ticket:** LPS-50604

#### What changed? [](id=what-changed-13)

The implementation class `com.liferay.portal.convert.ConvertProcess` was renamed
`com.liferay.portal.convert.BaseConvertProcess`. An interface named
`com.liferay.portal.convert.ConvertProcess` was created for it.

The `convert.processes` key was removed from `portal.properties`.
Consequentially, `ConvertProcess` implementations must register as OSGi
components.

#### Who is affected? [](id=who-is-affected-13)

This affects any implementations of the former `ConvertProcess` class, including
`ConvertProcess` class implementations in EXT plugins. Until version 6.2, this
type of service could only be implemented with an EXT plugin, given that the
`ConvertProcess` class resided in `portal-impl`.

#### How should I update my code? [](id=how-should-i-update-my-code-13)

You should replace `extends com.liferay.portal.convert.ConvertProcess` with
`extends com.liferay.portal.convert.BaseConvertProcess` and annotate the class
with `@Component(service=ConvertProcess.class)`.

Then turn your EXT plugin into an OSGi bundle and deploy it to the portal. You
should see your convert process in the configuration UI.

#### Why was this change made? [](id=why-was-this-change-made-13)

This change was made as a part of the ongoing strategy to modularize Liferay
Portal by means of an OSGi container.

---------------------------------------

### Migration of the Field Type from the Journal Article API into a Vocabulary [](id=migration-of-the-field-type-from-the-journal-article-api-into-a-vocabulary)
- **Date:** 2014-Oct-13
- **JIRA Ticket:** LPS-50764

#### What changed? [](id=what-changed-14)

The field *type* from the Journal Article entity has been removed. The Journal
API no longer supports this parameter. A new vocabulary called *Web Content
Types* is created when migrating from previous versions of Liferay, and the
types from the existing articles are kept as categories of this vocabulary.

#### Who is affected? [](id=who-is-affected-14)

This affects any caller of the removed methods `JournalArticle.getType()` and
`JournalFeed.getType()`, and callers of `ArticleTypeException`'s methods, that
attempt to use the former `type` parameter of the `JournalArticle` or
`JournalFeed` service.

#### How should I update my code? [](id=how-should-i-update-my-code-14)

If your logic was not affected by the type, you can simply remove the `type`
parameter from the Journal API call. If your logic was affected by the type, you
should now use the `AssetCategoryService` to obtain the category of the journal
articles.

#### Why was this change made? [](id=why-was-this-change-made-14)

Web Content Types had to be updated in a properties file and could not be
translated easily. Categories provide a much more flexible behavior and a better
UI. In addition, all the features, such as filters, developed for categories can
be used now in asset publishers and faceted search.

---------------------------------------

### Removed the getClassNamePortletId(String) Method from PortalUtil Class [](id=removed-the-getclassnameportletidstring-method-from-portalutil-class)
- **Date:** 2014-Nov-11
- **JIRA Ticket:** LPS-50604

#### What changed? [](id=what-changed-15)

The `getClassNamePortletId(String)` method from the `PortalUtil` class has been
removed.

#### Who is affected? [](id=who-is-affected-15)

This affects any plugin using the method.

#### How should I update my code? [](id=how-should-i-update-my-code-15)

If you are using the method, you should implement it yourself in a private
utility class.

#### Why was this change made? [](id=why-was-this-change-made-15)

This change was needed in order to modularize the portal. Also, the method is no
longer being used inside Liferay Portal.

---------------------------------------

### Removed the Header Web Content and Footer Web Content Preferences from the RSS Portlet [](id=removed-the-header-web-content-and-footer-web-content-preferences-from-the-)
- **Date:** 2014-Nov-12
- **JIRA Ticket:** LPS-46984

#### What changed? [](id=what-changed-16)

The *Header Web Content* and *Footer Web Content* preferences from the RSS
portlet have been removed. The portlet now supports Application Display
Templates (ADT), which provide templating capabilities that can apply web
content to the portlet's header and footer.

#### Who is affected? [](id=who-is-affected-16)

This affects RSS portlets that are displayed on pages and that use these
preferences. These preferences are no longer used in the RSS portlet.

#### How should I update my code? [](id=how-should-i-update-my-code-16)

Even though these preferences have been removed, an ADT can be created to
produce the same result. Liferay will publish this ADT so that it can be used in
the RSS portlet.

#### Why was this change made? [](id=why-was-this-change-made-16)

The support for ADTs in the RSS portlet not only covers this use case, but also
covers many other use cases, providing a much simpler way to create custom
preferences.

---------------------------------------

### Removed the createFlyouts Method from liferay/util.js and Related Resources [](id=removed-the-createflyouts-method-from-liferay-util-js-and-related-resources)
- **Date:** 2014-Dec-18
- **JIRA Ticket:** LPS-52275

#### What changed? [](id=what-changed-17)

The `Liferay.Util.createFlyouts` method has been completely removed from core
files.

#### Who is affected? [](id=who-is-affected-17)

This only affects third party developers who are explicitly calling
`Liferay.Util.createFlyouts` for the creation of flyout menus. It will not
affect any menus in core files.

#### How should I update my code? [](id=how-should-i-update-my-code-17)

If you are using the method, you can achieve the same behavior with CSS.

#### Why was this change made? [](id=why-was-this-change-made-17)

This method was removed due to there being no working use cases in Portal, and
its overall lack of functionality.

---------------------------------------

### Removed Support for Flat Thread View in Discussion Comments [](id=removed-support-for-flat-thread-view-in-discussion-comments)
- **Date:** 2014-Dec-30
- **JIRA Ticket:** LPS-51876

#### What changed? [](id=what-changed-18)

Discussion comments are now displayed using the *Combination* thread view, and
the number of levels displayed in the tree is limited.

#### Who is affected? [](id=who-is-affected-18)

This affects installations that specify portal property setting
`discussion.thread.view=flat`, which was the default setting.

#### How should I update my code? [](id=how-should-i-update-my-code-18)

There is no need to update anything since the portal property has been removed
and the `combination` thread view is now hard-coded.

#### Why was this change made? [](id=why-was-this-change-made-18)

Flat view comments were originally implemented as an option to tree view
comments, which were having performance issues with comment pagination.

Portal now uses a new pagination implementation that performs well. It allows
comments to display in a hierarchical view, making it easier to see reply
history. Therefore, the `flat` thread view is no longer needed.

---------------------------------------

### Removed Asset Tag Properties [](id=removed-asset-tag-properties)
- **Date:** 2015-Jan-13
- **JIRA Ticket:** LPS-52588

#### What changed? [](id=what-changed-19)

The *Asset Tag Properties* have been removed. The service no longer exists and
the Asset Tag Service API no longer has this parameter. The behavior associated
with tag properties in the Asset Publisher and XSL portlets has also been
removed.

#### Who is affected? [](id=who-is-affected-19)

This affects any plugin that uses the Asset Tag Properties service.

#### How should I update my code? [](id=how-should-i-update-my-code-19)

If you are using this functionality, you can achieve the same behavior with
*Asset Category Properties*. If you are using the Asset Tag Service, remove the
`String[]` tag properties parameter from your calls to the service's methods.

#### Why was this change made? [](id=why-was-this-change-made-19)

The Asset Tag Properties were deprecated for the 6.2 version of Liferay Portal.

---------------------------------------

### Removed the asset.publisher.asset.entry.query.processors Property [](id=removed-the-asset-publisher-asset-entry-query-processors-property)
- **Date:** 2015-Jan-22
- **JIRA Ticket:** LPS-52966

#### What changed? [](id=what-changed-20)

The `asset.publisher.asset.entry.query.processors` property has been removed
from `portal.properties`.

#### Who is affected? [](id=who-is-affected-20)

This affects any hook that uses the
`asset.publisher.asset.entry.query.processors` property.

#### How should I update my code? [](id=how-should-i-update-my-code-20)

If you are using this property to register Asset Entry Query Processors, your
Asset Entry Query Processor must implement the
`com.liferay.portlet.assetpublisher.util.AssetEntryQueryProcessor` interface and
must specify the `@Component(service=AssetEntryQueryProcessor.class)`
annotation.

#### Why was this change made? [](id=why-was-this-change-made-20)

This change was made as a part of the ongoing strategy to modularize Liferay
Portal.

---------------------------------------

### Replaced the ReservedUserScreenNameException with UserScreenNameException.MustNotBeReserved in UserLocalService [](id=replaced-the-reserveduserscreennameexception-with-userscreennameexception-m)
- **Date:** 2015-Jan-29
- **JIRA Ticket:** LPS-53113

#### What changed? [](id=what-changed-21)

Previous to Liferay 7, several methods of `UserLocalService` could throw a
`ReservedUserScreenNameException` when a user set a screen name that was not
allowed. That exception has been deprecated and replaced with
`UserScreenNameException.MustNotBeReserved`.

#### Who is affected? [](id=who-is-affected-21)

This affects developers who have written code that catches the
`ReservedUserScreenNameException` while calling the affected methods.

#### How should I update my code? [](id=how-should-i-update-my-code-21)

You should replace catching exception `ReservedUserScreenNameException` with
catching exception `UserScreenNameException.MustNotBeReserved`.

#### Why was this change made? [](id=why-was-this-change-made-21)

A new pattern has been defined for exceptions that provides higher expressivity
in their names and also more information regarding why the exception was thrown.

The new exception `UserScreenNameException.MustNotBeReserved` has all the
necessary information about why the exception was thrown and its context. In
particular, it contains the user ID, the problematic screen name, and the list
of reserved screen names.

---------------------------------------

### Replaced the ReservedUserEmailAddressException with UserEmailAddressException Inner Classes in User Services [](id=replaced-the-reserveduseremailaddressexception-with-useremailaddressexcepti)
- **Date:** 2015-Feb-03
- **JIRA Ticket:** LPS-53279

#### What changed? [](id=what-changed-22)

Previous to Liferay 7, several methods of `UserLocalService` and `UserService`
could throw a `ReservedUserEmailAddressException` when a user set an email
address that was not allowed. That exception has been deprecated and replaced
with `UserEmailAddressException.MustNotUseCompanyMx`,
`UserEmailAddressException.MustNotBePOP3User`, and
`UserEmailAddressException.MustNotBeReserved`.

#### Who is affected? [](id=who-is-affected-22)

This affects developers who have written code that catches the
`ReservedUserEmailAddressException` while calling the affected methods.

#### How should I update my code? [](id=how-should-i-update-my-code-22)

Depending on the method you're calling and the context in which you're calling
it, you should replace catching exception `ReservedUserEmailAddressException`
with catching exception `UserEmailAddressException.MustNotUseCompanyMx`,
`UserEmailAddressException.MustNotBePOP3User`, or
`UserEmailAddressException.MustNotBeReserved`.

#### Why was this change made? [](id=why-was-this-change-made-22)

A new pattern has been defined for exceptions. This pattern requires using
higher expressivity in exception names and requires that each exception provide
more information regarding why it was thrown.

Each new exception provides its context and has all the necessary information
about why the exception was thrown. For example, the
`UserEmailAddressException.MustNotBeReserved` exception contains the problematic
email address and the list of reserved email addresses.

---------------------------------------

### Replaced ReservedUserIdException with UserIdException Inner Classes [](id=replaced-reserveduseridexception-with-useridexception-inner-classes)
- **Date:** 2015-Feb-10
- **JIRA Ticket:** LPS-53487

#### What changed? [](id=what-changed-23)

The `ReservedUserIdException` has been deprecated and replaced with
`UserIdException.MustNotBeReserved`.

#### Who is affected? [](id=who-is-affected-23)

This affects developers who have written code that catches the
`ReservedUserIdException` while calling the affected methods.

#### How should I update my code? [](id=how-should-i-update-my-code-23)

You should replace catching exception `ReservedUserIdException` with
catching exception `UserIdException.MustNotBeReserved`.

#### Why was this change made? [](id=why-was-this-change-made-23)

A new pattern has been defined for exceptions that provides higher expressivity
in their names and also more information regarding why the exception was thrown.

The new exception `UserIdException.MustNotBeReserved` provides its context and
has all the necessary information about why the exception was thrown. In
particular, it contains the problematic user ID and the list of reserved user
IDs.

------------------------------------------------------------------------------

### Moved the AssetPublisherUtil Class and Removed It from the Public API [](id=moved-the-assetpublisherutil-class-and-removed-it-from-the-public-api)
- **Date:** 2015-Feb-11
- **JIRA Ticket:** LPS-52744

#### What changed? [](id=what-changed-24)

The class `AssetPublisherUtil` from the `portal-service` module has been moved
to the module `AssetPublisher` and it is no longer a part of the public API.

#### Who is affected? [](id=who-is-affected-24)

This affects developers who have written code that uses the `AssetPublisherUtil`
class.

#### How should I update my code? [](id=how-should-i-update-my-code-24)

This `AssetPublisherUtil` class should no longer be used from other modules
since it contains utility methods for the Asset Publisher portlet. If needed,
you can define a dependency with the Asset Publisher module and use the new
class.

#### Why was this change made? [](id=why-was-this-change-made-24)

This change has been made as part of the modularization efforts to decouple the
different parts of the portal.

---------------------------------------

### Removed Operations That Used the Fields Class from the StorageAdapter Interface [](id=removed-operations-that-used-the-fields-class-from-the-storageadapter-inter)
- **Date:** 2015-Feb-11
- **JIRA Ticket:** LPS-53021

#### What changed? [](id=what-changed-25)

All operations that used the `Fields` class have been removed from the
`StorageAdapter` interface.

#### Who is affected? [](id=who-is-affected-25)

This affects developers who have written code that directly calls these
operations.

#### How should I update my code? [](id=how-should-i-update-my-code-25)

You should update your code to use the `DDMFormValues` class instead of the
`Fields` class.

#### Why was this change made? [](id=why-was-this-change-made-25)

This change has been made due to the deprecation of the `Fields` class.

---------------------------------------

### Created a New getType Method That is Implemented in DLProcessor [](id=created-a-new-gettype-method-that-is-implemented-in-dlprocessor)
- **Date:** 2015-Feb-17
- **JIRA Ticket:** LPS-53574

#### What changed? [](id=what-changed-26)

The `DLProcessor` interface has a new method `getType()`.

#### Who is affected? [](id=who-is-affected-26)

This affects developers who have created a `DLProcessor`.

#### How should I update my code? [](id=how-should-i-update-my-code-26)

You should implement the new method and return the type of processor. You can
check the class `DLProcessorConstants` to see processor types.

#### Why was this change made? [](id=why-was-this-change-made-26)

Previous to Liferay 7, developers were forced to extend one of the existing
`DLProcessor` classes and developers using the extended class had to check the
instance of that class to determine its processor type.

With this change, developers no longer need to extend any particular class to
create their own `DLProcessor` and their processor's type can be clearly
specified by a constant from the class `DLProcessorConstants`.

---------------------------------------

### Changed the Usage of the liferay-ui:restore-entry Tag [](id=changed-the-usage-of-the-liferay-uirestore-entry-tag)
- **Date:** 2015-Mar-01
- **JIRA Ticket:** LPS-54106

#### What changed? [](id=what-changed-27)

The usage of the taglib tag `liferay-ui:restore-entry` serves a different
purpose now. It renders the UI to restore elements from the Recycle Bin.

#### Who is affected? [](id=who-is-affected-27)

This affects developers using the tag `liferay-ui:restore-entry`.

#### How should I update my code? [](id=how-should-i-update-my-code-27)

You should replace your calls to the tag with code like the listing below:

    <aui:script use="liferay-restore-entry">
        new Liferay.RestoreEntry(
        {
                checkEntryURL: '<%= checkEntryURL.toString() %>',
                duplicateEntryURL: '<%= duplicateEntryURL.toString() %>',
                namespace: '<portlet:namespace />'
            }
        );
    </aui:script>

In the above code, the `checkEntryURL` should be an `ActionURL` of your portlet,
which checks whether the current entry can be restored from the Recycle Bin. The
`duplicateEntryURL` should be a `RenderURL` of your portlet, that renders the UI
to restore the entry, resolving any existing conflicts. In order to generate
that URL, you can use the tag `liferay-ui:restore-entry`, which has been
refactored for this usage.

#### Why was this change made? [](id=why-was-this-change-made-27)

This change allows the Trash portlet to be an independent module. Its actions
and views are no longer used by the tag; they are now the responsability of
each plugin.

---------------------------------------

### Added Required Parameter resourceClassNameId for DDM Template Search Operations [](id=added-required-parameter-resourceclassnameid-for-ddm-template-search-operat)
- **Date:** 2015-Mar-03
- **JIRA Ticket:** LPS-52990

#### What changed? [](id=what-changed-28)

The DDM template `search` and `searchCount` operations have a new parameter
called `resourceClassNameId`.

#### Who is affected? [](id=who-is-affected-28)

This affects developers who have direct calls to the `DDMTemplateService` or
`DDMTemplateLocalService`.

#### How should I update my code? [](id=how-should-i-update-my-code-28)

You should add the `resourceClassNameId` parameter to your calls. This parameter
represents the resource that owns the permission for the DDM template. For
example, if the template is a WCM template, the `resourceClassNameId` points to
the `JournalArticle`'s `classNameId`. If the template is a DDL template, the
`resourceClassNameId` points to the `DDLRecordSet`'s `classNameId`. If the
template is an ADT template, the `resourceClassNameId` points to the
`PortletDisplayTemplate`'s `classNameId`.

#### Why was this change made? [](id=why-was-this-change-made-28)

This change was made in order to implement model resource permissions for DDM
templates, such as `VIEW`, `DELETE`, `PERMISSIONS`, and `UPDATE`.

---------------------------------------

### Replaced the Breadcrumb Portlet's Display Styles with ADTs [](id=replaced-the-breadcrumb-portlets-display-styles-with-adts)
- **Date:** 2015-Mar-12
- **JIRA Ticket:** LPS-53577

#### What changed? [](id=what-changed-29)

The custom display styles of the breadcrumb tag added using JSPs no longer work.
They have been replaced by Application Display Templates (ADT).

#### Who is affected? [](id=who-is-affected-29)

This affects developers that use the following properties:

    breadcrumb.display.style.default=horizontal

    breadcrumb.display.style.options=horizontal,vertical

#### How should I update my code? [](id=how-should-i-update-my-code-29)

To style the Breadcrumb portlet, you should use ADTs instead of using custom
styles in your JSPs. ADTs can be created from the UI of the portal by navigating
to *Site Settings* &rarr; *Application Display Templates*. ADTs can also be
created programatically.

#### Why was this change made? [](id=why-was-this-change-made-29)

ADTs allow you to change an application's look and feel without changing its JSP
code.

---------------------------------------

### Changed Usage of the liferay-ui:ddm-template-selector Tag [](id=changed-usage-of-the-liferay-uiddm-template-selector-tag)
- **Date:** 2015-Mar-16
- **JIRA Ticket:** LPS-53790

#### What changed? [](id=what-changed-30)

The attribute `classNameId` of the `liferay-ui:ddm-template-selector` taglib tag
has been renamed `className`.

#### Who is affected? [](id=who-is-affected-30)

This affects developers using the `liferay-ui:ddm-template-selector` tag.

#### How should I update my code? [](id=how-should-i-update-my-code-30)

In your `liferay-ui:ddm-template-selector` tags, rename the `classNameId`
attribute to `className`.

#### Why was this change made? [](id=why-was-this-change-made-30)

Application Display Templates were being referenced by their UUID, which was
usually not known by the developer. Referencing all DDM templates by their class
name simplifies using this tag.

---------------------------------------

### Changed the Usage of Asset Preview [](id=changed-the-usage-of-asset-preview)
- **Date:** 2015-Mar-16
- **JIRA Ticket:** LPS-53972

#### What changed? [](id=what-changed-31)

Instead of directly including the JSP referenced by the `AssetRenderer`'s
`getPreviewPath` method to preview an asset, you now use a taglib tag.

#### Who is affected? [](id=who-is-affected-31)

This affects developers who have written code that directly calls an
`AssetRenderer`'s `getPreviewPath` method to preview an asset.

#### How should I update my code? [](id=how-should-i-update-my-code-31)

JSP code that previews an asset by calling an `AssetRenderer`'s `getPreviewPath`
method, such as in the example code below, must be replaced:

    <liferay-util:include
        page="<%= assetRenderer.getPreviewPath(liferayPortletRequest, liferayPortletResponse) %>"
        portletId="<%= assetRendererFactory.getPortletId() %>"
        servletContext="<%= application %>"
    />

To preview an asset, you should instead use the `liferay-ui:asset-display` tag,
passing it an instance of the asset entry and an asset renderer preview
template. Here's an example of using the tag:

    <liferay-ui:asset-display
        assetEntry="<%= assetEntry %>"
        template="<%= AssetRenderer.TEMPLATE_PREVIEW %>"
    />

#### Why was this change made? [](id=why-was-this-change-made-31)

This change simplifies using asset previews.

---------------------------------------

### Added New Methods in the ScreenNameValidator Interface [](id=added-new-methods-in-the-screennamevalidator-interface)
- **Date:** 2015-Mar-17
- **JIRA Ticket:** LPS-53409

#### What changed? [](id=what-changed-32)

The `ScreenNameValidator` interface has new methods `getDescription(Locale)` and
`getJSValidation()`.

#### Who is affected? [](id=who-is-affected-32)

This affects developers who have implemented a custom screen name validator with
the `ScreenNameValidator` interface.

#### How should I update my code? [](id=how-should-i-update-my-code-32)

You should implement the new methods introduced in the interface.

- `getDescription(Locale)`: returns a description of what the screen name
validator validates.

- `getJSValidation()`: returns the JavaScript input validator on the client
side.

#### Why was this change made? [](id=why-was-this-change-made-32)

Previous to Liferay 7, validation for user screen name characters was hard-coded
in `UserLocalService`. A new portal property named
`users.screen.name.special.characters` has been added to provide configurability
of special characters allowed in screen names.

In addition, developers can now specify a custom input validator for the screen
name on the client side by providing a JavaScript validator in
`getJSValidation()`.

---------------------------------------

### Replaced the Language Portlet's Display Styles with ADTs [](id=replaced-the-language-portlets-display-styles-with-adts)
- **Date:** 2015-Mar-30
- **JIRA Ticket:** LPS-54419

#### What changed? [](id=what-changed-33)

The custom display styles of the language tag added using JSPs no longer work.
They have been replaced by Application Display Templates (ADT).

#### Who is affected? [](id=who-is-affected-33)

This affects developers that use the following properties:

    language.display.style.default=icon

    language.display.style.options=icon,long-text

#### How should I update my code? [](id=how-should-i-update-my-code-33)

To style the Language portlet, you should use ADTs instead of using custom
styles in your JSPs. ADTs can be created from the UI of the portal by navigating
to *Site Settings* &rarr; *Application Display Templates*. ADTs can also be
created programatically.

#### Why was this change made? [](id=why-was-this-change-made-33)

ADTs allow you to change an application's look and feel without changing its JSP
code.

---------------------------------------

### Added Required Parameter groupId for Adding Tags, Categories, and Vocabularies [](id=added-required-parameter-groupid-for-adding-tags-categories-and-vocabularie)
- **Date:** 2015-Mar-31
- **JIRA Ticket:** LPS-54570

#### What changed? [](id=what-changed-34)

The API for adding tags, categories, and vocabularies now requires passing the
`groupId` parameter. Previously, it had to be included in the `ServiceContext`
parameter passed to the method.

#### Who is affected? [](id=who-is-affected-34)

This affects developers who have direct calls to the following methods:

- `addTag` in `AssetTagService` or `AssetTagLocalService`
- `addCategory` in `AssetCategoryService` or `AssetCategoryLocalService`
- `addVocabulary` in `AssetVocabularyService` or `AssetVocabularyLocalService`
- `updateFolder` in `JournalFolderService` or `JournalFolderLocalService`

#### How should I update my code? [](id=how-should-i-update-my-code-34)

You should add the `groupId` parameter to your calls. This parameter represents
the site in which you are creating the tag, category, or vocabulary. It can be
obtained from the `themeDisplay` or `serviceContext` using
`themeDisplay.getScopeGroupId()` or `serviceContext.getScopeGroupId()`,
respectively.

#### Why was this change made? [](id=why-was-this-change-made-34)

This change was made in order improve the API. The `groupId` parameter was
always required, but it was hidden by the `ServiceContext` object.

---------------------------------------

### Removed the Tags that Start with portlet:icon- [](id=removed-the-tags-that-start-with-portleticon-)
- **Date:** 2015-Mar-31
- **JIRA Ticket:** LPS-54620

#### What changed? [](id=what-changed-35)

The following tags have been removed:

- `portlet:icon-close`
- `portlet:icon-configuration`
- `portlet:icon-edit`
- `portlet:icon-edit-defaults`
- `portlet:icon-edit-guest`
- `portlet:icon-export-import`
- `portlet:icon-help`
- `portlet:icon-maximize`
- `portlet:icon-minimize`
- `portlet:icon-portlet-css`
- `portlet:icon-print`
- `portlet:icon-refresh`
- `portlet:icon-staging`

#### Who is affected? [](id=who-is-affected-35)

This affects developers who have written code that uses these tags.

#### How should I update my code? [](id=how-should-i-update-my-code-35)

The tag `liferay-ui:icon` can replace the call to the previous tags. All the
previous tags have been converted into Java classes that implement the methods
that the `icon` tag requires.

See the modules `portlet-configuration-icon-*` in the `modules/addons` folder.

#### Why was this change made? [](id=why-was-this-change-made-35)

These tags were used to generate the configuration icon of portlets. This
functionality will now be managed from OSGi modules instead of tags since OSGi
modules provide more flexibility and can be included in any app.

---------------------------------------

### Changed the Default Value of the copy-request-parameters Init Parameter for MVC Portlets [](id=changed-the-default-value-of-the-copy-request-parameters-init-parameter-for)
- **Date:** 2015-Apr-15
- **JIRA Ticket:** LPS-54798

#### What changed? [](id=what-changed-36)

The `copy-request-parameters` init parameter's default value is now set to
`true` in all portlets that extend `MVCPortlet`.

#### Who is affected? [](id=who-is-affected-36)

This affects developers that have created portlets that extend `MVCPortlet`.

#### How should I update my code? [](id=how-should-i-update-my-code-36)

To continue using the property the same way you did before this change was
implemented, you'll need to change the default property. To change the property,
set the init parameter to `false` in your class extending `MVCPortlet`:

    javax.portlet.init-param.copy-request-parameters=false

#### Why was this change made? [](id=why-was-this-change-made-36)

This change was made to allow for backwards compatibility.

---------------------------------------

### Removed Portal Properties Used to Display Sections in Form Navigators [](id=removed-portal-properties-used-to-display-sections-in-form-navigators)
- **Date:** 2015-Apr-16
- **JIRA Ticket:** LPS-54903

#### What changed? [](id=what-changed-37)

The following portal properties (and the equivalent `PropsKeys` and
`PropsValues`) that were used to decide what sections would be displayed in the
`form-navigator` have been removed:

- `company.settings.form.configuration`
- `company.settings.form.identification`
- `company.settings.form.miscellaneous`
- `company.settings.form.social`
- `layout.form.add`
- `layout.form.update`
- `layout.set.form.update`
- `organizations.form.add.identification`
- `organizations.form.add.main`
- `organizations.form.add.miscellaneous`
- `organizations.form.update.identification`
- `organizations.form.update.main`
- `organizations.form.update.miscellaneous`
- `sites.form.add.advanced`
- `sites.form.add.main`
- `sites.form.add.miscellaneous`
- `sites.form.add.seo`
- `sites.form.update.advanced`
- `sites.form.update.main`
- `sites.form.update.miscellaneous`
- `sites.form.update.seo`
- `users.form.add.identification`
- `users.form.add.main`
- `users.form.add.miscellaneous`
- `users.form.my.account.identification`
- `users.form.my.account.main`
- `users.form.my.account.miscellaneous`
- `users.form.update.identification`
- `users.form.update.main`
- `users.form.update.miscellaneous`

The sections and categories of form navigators are now OSGi components.

#### Who is affected? [](id=who-is-affected-37)

This affects administrators who may have added, removed, or reordered sections
using those portal properties. Developers using the constants defined in
`PropsKeys` or `PropsValues` for those portal properties will also be affected.

#### How should I update my code? [](id=how-should-i-update-my-code-37)

Since those properties no longer exist, you cannot rely on them. References to
the constants of `PropsKeys` and `PropsValues` will need to be updated. You can
use `FormNavigatorCategoryUtil` and `FormNavigatorEntryUtil` to obtain a list of
the available sections and categories for a form navigator instance.

Changes to remove or reorder specific sections will need to be done through the
OSGi console to update the service ranking or stop the components.

Adding new sections with Liferay Hooks will still work as a legacy feature, but
the recommended way is using OSGi components to add new sections.

#### Why was this change made? [](id=why-was-this-change-made-37)

The old mechanism to add new sections to `form-navigator` tags was very
limited because it could only depend on portal for services and utils due to the
new section that was rendered from the portal classloader.

There was a need to add new sections and categories to `form-navigator` tags via
OSGi plugins in a more extensible way, allowing the developer to include new
sections to access to their own utils and services.

---------------------------------------

### Removed the Type Setting breadcrumbShowParentGroups from Groups [](id=removed-the-type-setting-breadcrumbshowparentgroups-from-groups)
- **Date:** 2015-Apr-21
- **JIRA Ticket:** LPS-54791

#### What changed? [](id=what-changed-38)

The type setting `breadcrumbShowParentGroups` was removed from groups and is
no longer available in the site configuration. Now, it is only available in the
breadcrumb configuration.

#### Who is affected? [](id=who-is-affected-38)

This affects all site administrators that have set the `showParentGroups`
preference in Site Administration.

#### How should I update my code? [](id=how-should-i-update-my-code-38)

There are no code updates required. This should only be updated at the portlet
instance level.

#### Why was this change made? [](id=why-was-this-change-made-38)

This change was introduced to support the new Settings API.

---------------------------------------

### Changed Return Value of the Method getText of the Editor's Window API [](id=changed-return-value-of-the-method-gettext-of-the-editors-window-api)
- **Date:** 2015-Apr-28
- **JIRA Ticket:** LPS-52698

#### What changed? [](id=what-changed-39)

The method `getText` now returns the editor's content, without any HTML markup.

#### Who is affected? [](id=who-is-affected-39)

This affects developers that are using the `getText` method of the editor's
window API.

#### How should I update my code? [](id=how-should-i-update-my-code-39)

To continue using the editor the same way you did before this change was
implemented, you should change calls to the `getText` method to instead call the
`getHTML` method.

#### Why was this change made? [](id=why-was-this-change-made-39)

This change was made in the editor's window API to provide a proper `getText`
method that returns just the editor's content, without any HTML markup. This
change is used for the blog abstract field.

---------------------------------------

### Moved the Contact Name Exception Classes to Inner Classes of ContactNameException [](id=moved-the-contact-name-exception-classes-to-inner-classes-of-contactnameexc)
- **Date:** 2015-May-05
- **JIRA Ticket:** LPS-55364

#### What changed? [](id=what-changed-40)

The use of classes `ContactFirstNameException`, `ContactFullNameException`, and
`ContactLastNameException` has been moved to inner classes in a new class called
`ContactNameException`.

#### Who is affected? [](id=who-is-affected-40)

This affects developers who may have included one of the three classes above in
their code.

#### How should I update my code? [](id=how-should-i-update-my-code-40)

While the old classes remain for backwards-compatibility, they are being
deprecated. You're encouraged to use the new pattern of inner classes for
exceptions wherever possible. For example, instead of using
`ContactFirstNameExeception`, use `ContactNameException.MustHaveFirstName`.

#### Why was this change made? [](id=why-was-this-change-made-40)

This change was made in accordance with the new exceptions pattern being applied
throughout Portal. It also allows the new localized user name configuration
feature to be thoroughly covered by exceptions for different configurations.  

---------------------------------------

### Removed USERS_LAST_NAME_REQUIRED from portal.properties in Favor of language.properties Configurations [](id=removed-userslastnamerequired-from-portal-properties-in-favor-of-language-p)
- **Date:** 2015-May-07
- **JIRA Ticket:** LPS-54956

#### What changed? [](id=what-changed-41)

The `USERS_LAST_NAME_REQUIRED` property has been removed from
`portal.properties` and the corresponding UI. Required names are now handled on
a per-language basis via the `language.properties` files. It has also been
removed as an option from the Portal Settings section of the Control Panel.

#### Who is affected? [](id=who-is-affected-41)

This affects anyone who uses the `USERS_LAST_NAME_REQUIRED` portal property.

#### How should I update my code? [](id=how-should-i-update-my-code-41)

If you need to require the user's last name, list it on the
`lang.user.name.required.field.names` line of the appropriate
`language.properties` files:

    lang.user.name.required.field.names=last-name

#### Why was this change made? [](id=why-was-this-change-made-41)

Portal property `USERS_LAST_NAME_REQUIRED` didn't support the multicultural user
name configurations introduced in LPS-48406. Language property files (e.g.,
`language.properties`) now support these configurations. Control of all user
name configuration, except with regards to first name, is relegated to language
property files. First name is required and always present.

---------------------------------------

### Removed Methods getGroupLocalRepositoryImpl and getLocalRepositoryImpl from RepositoryLocalService and RepositoryService [](id=removed-methods-getgrouplocalrepositoryimpl-and-getlocalrepositoryimpl-from)
- **Date:** 2015-May-14
- **JIRA Ticket:** LPS-55566

#### What changed? [](id=what-changed-42)

The methods `getGroupLocalRepositoryImpl(...)` and `getLocalRepositoryImpl(...)`
have been removed from `RepositoryLocalService` and `RepositoryService`.
Although the methods are related to the service, they belong in a different
level of abstraction.

#### Who is affected? [](id=who-is-affected-42)

This affects anyone who uses those methods.

#### How should I update my code? [](id=how-should-i-update-my-code-42)

The removed methods were generic and had long signatures with optional
parameters. They now have one specialized version per parameter and are
in the `RepositoryProvider` service.

**Example**

Old call:

    RepositoryLocalServiceUtil.getRepositoryImpl(0, fileEntryId, 0)

New call:

    RepositoryProviderUtil.getLocalRepositoryByFileEntryId(fileEntryId)

#### Why was this change made? [](id=why-was-this-change-made-42)

This change was made to enhance the Repository API and facilitate decoupling the
API from the Document Library, as a part of the portal modularization effort.

---------------------------------------

### Removed addFileEntry Method from DLAppHelperLocalService [](id=removed-addfileentry-method-from-dlapphelperlocalservice)
- **Date:** 2015-May-20
- **JIRA Ticket:** LPS-47645

#### What changed? [](id=what-changed-43)

The `addFileEntry` method has been removed from `DLAppHelperLocalService`.

#### Who is affected? [](id=who-is-affected-43)

This affects anyone who calls the `addFileEntry` method.

#### How should I update my code? [](id=how-should-i-update-my-code-43)

If you need to invoke the `addFileEntry` method as part of a custom repository
implementation, use the provided repository capabilities instead. See
`LiferayRepositoryDefiner` for examples on their use.

For other use cases, you may need to explicitly invoke each of the service
methods used by `addFileEntry`.

#### Why was this change made? [](id=why-was-this-change-made-43)

The logic inside the `addFileEntry` method was moved, out from
`DLAppHelperLocalService` and into repository capabilities, to further decouple
core repository implementations from additional (optional) functionality.

---------------------------------------

### Indexers Called from Document Library Now Receive FileEntry Instead of DLFileEntry [](id=indexers-called-from-document-library-now-receive-fileentry-instead-of-dlfi)
- **Date:** 2015-May-20
- **JIRA Ticket:** LPS-55613

#### What changed? [](id=what-changed-44)

Indexers that previously received a `DLFileEntry` object (e.g., in the
`addRelatedEntryFields` method) no longer receive a `DLFileEntry`, but a
`FileEntry`.

#### Who is affected? [](id=who-is-affected-44)

This affects anyone who implements an Indexer handling `DLFileEntry` objects.

#### How should I update my code? [](id=how-should-i-update-my-code-44)

You should try to use methods in `FileEntry` or exported repository capabilities
to obtain the value you were using. If no capability exists for your use case,
you can resort to calling `fileEntry.getModel()` and casting the result to a
`DLFileEntry`. However, this breaks all encapsulation and may result in future
failures or compatibility problems.

Old code:

    @Override
    public void addRelatedEntryFields(Document document, Object obj)
        throws Exception {

        DLFileEntry dlFileEntry = (DLFileEntry)obj;

        long fileEntryId = dlFileEntry.getFileEntryId();

New Code:

    @Override
    public void addRelatedEntryFields(Document document, Object obj)
        throws Exception {

        FileEntry fileEntry = (FileEntry)obj;

        long fileEntryId = fileEntry.getFileEntryId();

#### Why was this change made? [](id=why-was-this-change-made-44)

This change was made to enhance the Repository API and make decoupling from
Document Library easier when modularizing the portal.

---------------------------------------

### Removed permissionClassName, permissionClassPK, and permissionOwner Parameters from MBMessage API [](id=removed-permissionclassname-permissionclasspk-and-permissionowner-parameter)
- **Date:** 2015-May-27
- **JIRA Ticket:** LPS-55877

#### What changed? [](id=what-changed-45)

The parameters `permissionClassName`, `permissionClassPK`, and `permissionOwner`
have been removed from the Message Boards API and Discussion tag.

#### Who is affected? [](id=who-is-affected-45)

This affects anyone who invokes the affected methods (locally or remotely) and
any view that uses the Discussion tag.

#### How should I update my code? [](id=how-should-i-update-my-code-45)

It suffices to remove the parameters from the method calls (for consumers of the
API) or the attributes in tag invocations.

#### Why was this change made? [](id=why-was-this-change-made-45)

Those API methods were exposed in the remote services, allowing any consumer to
bypass the permission system by providing customized `className`, `classPK`, or
`ownerId` parameters.

---------------------------------------

### Moved Indexer.addRelatedEntryFields and Indexer.reindexDDMStructures, and Removed Indexer.getQueryString [](id=moved-indexer-addrelatedentryfields-and-indexer-reindexddmstructures-and-re)
- **Date:** 2015-May-27
- **JIRA Ticket:** LPS-55928

#### What changed? [](id=what-changed-46)

Method `Indexer.addRelatedEntryFields(Document, Object)` has been moved into
`RelatedEntryIndexer`.

`Indexer.reindexDDMStructures(List<Long>)` has been moved into
`DDMStructureIndexer`.

`Indexer.getQueryString(SearchContext, Query)` has been removed, in favor of
calling `SearchEngineUtil.getQueryString(SearchContext, Query)`

#### Who is affected? [](id=who-is-affected-46)

This affects any code that invokes the affected methods, as well as any code
that implements the interface methods.

#### How should I update my code? [](id=how-should-i-update-my-code-46)

Any code implementing `Indexer.addRelatedEntryFields(...)` should implement the
`RelatedEntryIndexer` interface.

Any code calling `Indexer.addRelatedEntryFields(...)` should determine first if
the `Indexer` is an instance of `RelatedEntryIndexer`.

Old code:

    mbMessageIndexer.addRelatedEntryFields(...);

New code:

    if (mbMessageIndexer instanceof RelatedEntryIndexer) {
        RelatedEntryIndexer relatedEntryIndexer =
            (RelatedEntryIndexer)mbMessageIndexer;

        relatedEntryIndexer.addRelatedEntryFields(...);
    }

Any code implementing `Indexer.reindexDDMStructures(...)` should implement the
`DDMStructureIndexer` interface.

Any code calling `Indexer.reindexDDMStructures(...)` should determine first if
the `Indexer` is an instance of `DDMStructureIndexer`.

Old code:

    mbMessageIndexer.reindexDDMStructures(...);

New code:

    if (journalIndexer instanceof DDMStructureIndexer) {
        DDMStructureIndexer ddmStructureIndexer =
            (DDMStructureIndexer)journalIndexer;

        ddmStructureIndexer.reindexDDMStructures(...);
    }

Any code calling `Indexer.getQueryString(...)` should call
`SearchEngineUtil.getQueryString(...)`.

Old code:

    mbMessageIndexer.getQueryString(...);

New code:

    SearchEngineUtil.getQueryString(...);

#### Why was this change made? [](id=why-was-this-change-made-46)

The `addRelatedEntryFields` and `reindexDDMStructures` methods were not related
to core indexing functions. They were functions of specialized indexers.

The `getQueryString` method was an unnecessary convenience method.

---------------------------------------

### Removed mbMessages and fileEntryTuples Attributes from app-view-search-entry Tag [](id=removed-mbmessages-and-fileentrytuples-attributes-from-app-view-search-entr)
- **Date:** 2015-May-27
- **JIRA Ticket:** LPS-55886

#### What changed? [](id=what-changed-47)

The `mbMessages` and `fileEntryTuples` attributes from the
`app-view-search-entry` tag have been removed. Related methods `getMbMessages`,
`getFileEntryTuples`, and `addMbMessage` have also been removed from the
`SearchResult` class.

#### Who is affected? [](id=who-is-affected-47)

This affects developers that use the `app-view-search-entry` tag in their views,
have developed hooks to customize the tag JSP, or have developed a portlet that
uses that tag. Also, any custom code that uses the `SearchResult` class may be
affected.

#### How should I update my code? [](id=how-should-i-update-my-code-47)

The new attributes `commentRelatedSearchResults` and
`fileEntryRelatedSearchResults` should be used instead. The expected value is
the one returned by the `getCommentRelatedSearchResults` and
`getFileEntryRelatedSearchResults` methods in `SearchResult`.

When adding comments to the `SearchResult`, the new `addComment` method should
be used instead of the `addMbMessage` method.

#### Why was this change made? [](id=why-was-this-change-made-47)

As part of the modularization efforts, references to `MBMessage` needed to be
removed for the Message Boards portlet to be placed into its own OSGi bundle.

---------------------------------------

### Replaced Method getPermissionQuery with getPermissionFilter in SearchPermissionChecker, and getFacetQuery with getFacetBooleanFilter in Indexer [](id=replaced-method-getpermissionquery-with-getpermissionfilter-in-searchpermis)
- **Date:** 2015-Jun-02
- **JIRA Ticket:** LPS-56064

#### What changed? [](id=what-changed-48)

Method `SearchPermissionChecker.getPermissionQuery(
long, long[], long, String, Query, SearchContext)`
has been replaced by `SearchPermissionChecker.getPermissionBooleanFilter(
long, long[], long, String, BooleanFilter, SearchContext)`.

Method `Indexer.getFacetQuery(String, SearchContext)` has been replaced by
`Indexer.getFacetBooleanFilter(String, SearchContext)`.

#### Who is affected? [](id=who-is-affected-48)

This affects any code that invokes the affected methods, as well as any code
that implements the interface methods.

#### How should I update my code? [](id=how-should-i-update-my-code-48)

Any code calling/implementing `SearchPermissionChecker.getPermissionQuery(...)`
should instead call/implement
`SearchPermissionChecker.getPermissionBooleanFilter(...)`.

Any code calling/implementing `Indexer.getFacetQuery(...)` should instead
call/implement `Indexer.getFacetBooleanFilter(...)`.

#### Why was this change made? [](id=why-was-this-change-made-48)

Permission constraints placed on search should not affect the score for returned
search results.  Thus, these constraints should be applied as search filters.
`SearchPermissionChecker` is also a very deep internal interface within the
permission system.  Thus, to limit confusion in the logic for maintainability,
the `SearchPermissionChecker.getPermissionQuery(...)` method was removed as
opposed to deprecated.

Similarly, constraints applied to facets should not affect the scoring or facet
counts. Since `Indexer.getFacetQuery(...)` was only utilized by the
`AssetEntriesFacet`, and used to reduce the impact of changes for
`SearchPermissionChecker.getPermissionBooleanFilter(...)`, the method was
removed as opposed to deprecated.

---------------------------------------

### Added userId Parameter to Update Operations of DDMStructureLocalService and DDMTemplateLocalService [](id=added-userid-parameter-to-update-operations-of-ddmstructurelocalservice-and)
- **Date:** 2015-Jun-05
- **JIRA Ticket:** LPS-50939

#### What changed? [](id=what-changed-49)

A new parameter `userId` has been added to the `updateStructure` and
`updateTemplate` methods of the `DDMStructureLocalService` and
`DDMTemplateLocalService` classes, respectively.

#### Who is affected? [](id=who-is-affected-49)

This affects any code that invokes the affected methods, as well as any code
that implements the interface methods.

#### How should I update my code? [](id=how-should-i-update-my-code-49)

Any code calling/implementing
`DDMStructureLocalServiceUtil.updateStructure(...)` or
`DDMTemplateLocalServiceUtil.updateTemplate(...)` should pass the new `userId`
parameter.

#### Why was this change made? [](id=why-was-this-change-made-49)

For the service to keep track of which user is modifying the structure or
template, the `userId` parameter was required. In order to add support to
structure and template versions, audit columns were also added to such models.

---------------------------------------

### Removed Method getEntries from DL, DLImpl, and DLUtil Classes [](id=removed-method-getentries-from-dl-dlimpl-and-dlutil-classes)
- **Date:** 2015-Jun-10
- **JIRA Ticket:** LPS-56247

#### What changed? [](id=what-changed-50)

The method `getEntries` has been removed from the `DL`, `DLImpl`, and `DLUtil`
classes.

#### Who is affected? [](id=who-is-affected-50)

This affects any caller of the `getEntries` method.

#### How should I update my code? [](id=how-should-i-update-my-code-50)

You may use the `SearchResultUtil` class to process the search results. Note
that this class is not completely equivalent; if you need exactly the same
behavior as the removed method, you will need to add custom code.

#### Why was this change made? [](id=why-was-this-change-made-50)

The `getEntries` method was no longer used, and contained hardcoded references
to classes that will be moved into OSGi bundles.

---------------------------------------

### Removed WikiUtil.getEntries Method [](id=removed-wikiutil-getentries-method)
- **Date:** 2015-Jun-10
- **JIRA Ticket:** LPS-56242

#### What changed? [](id=what-changed-51)

The method `getEntries()` has been removed from class `WikiUtil`.

#### Who is affected? [](id=who-is-affected-51)

Any JSP hook or ext plugin that uses this method is affected. As the class was
located in portal-impl, regular portlets and other safe extension points won't
be affected.

#### How should I update my code? [](id=how-should-i-update-my-code-51)

You should review the JSP or ext plugin, updating it to remove any reference to
the new class and mimicking the original JSP code. In case you need equivalent
functionality to the one provided by `WikiUtil.getEntries()` you may use the
`SearchResultUtil` class. While not totally equivalent, it offers similar
functionality.

#### Why was this change made? [](id=why-was-this-change-made-51)

The `WikiUtil.getEntries()` method was no longer used, and it contained
hardcoded references to classes that will be moved into OSGi modules.

---------------------------------------

### Removed render Method from ConfigurationAction API [](id=removed-render-method-from-configurationaction-api)
- **Date:** 2015-Jun-14
- **JIRA Ticket:** LPS-56300

#### What changed? [](id=what-changed-52)

The method `render` has been removed from the interface `ConfigurationAction`.

#### Who is affected? [](id=who-is-affected-52)

This affects any Java code calling the method `render` on a
`ConfigurationAction` class, or Java classes overriding the `render` method of a
`ConfigurationAction` class.

#### How should I update my code? [](id=how-should-i-update-my-code-52)

The method `render` was used to return the path of a JSP, including the
configuration of a portlet. That method is now available for configurations
extending the `BaseJSPSettingsConfigurationAction` class, and is called
`getJspPath`.

If any logic was added to override the `render` method, it can now be added in
the `include` method.

#### Why was this change made? [](id=why-was-this-change-made-52)

This change was part of needed modifications to support adding configuration for
portlets based on other technology different than JSP (e.g., FreeMarker). The
method `include` can now be used to create configuration UIs written in
FreeMarker or any other framework.

---------------------------------------

### Removed ckconfig Files Used for CKEditor Configuration [](id=removed-ckconfig-files-used-for-ckeditor-configuration)
- **Date:** 2015-Jun-16
- **JIRA Ticket:** LPS-55518

#### What changed? [](id=what-changed-53)

The files `ckconfig.jsp`, `ckconfig-ext.jsp`, `ckconfig_bbcode.jsp`,
`ckconfig_bbcode-ext.jsp`, `ckconfig_creole.jsp`, and `ckconfig_creole-ext.jsp`
have been removed and are no longer used to configure the CKEditor instances
created using the `liferay-ui:input-editor` tag.

#### Who is affected? [](id=who-is-affected-53)

This affects any hook or plugin-ext overriding these files to modify the editor
configuration.

#### How should I update my code? [](id=how-should-i-update-my-code-53)

Depending on the changes, different extension methods are available:

- For CKEditor configuration options, an implementation of
`EditorConfigContributor` can be created to pass or modify the expected
parameters.
- For CKEditor instance manipulation (setting attributes, adding listeners,
etc.), the `DynamicInclude` extension point
`#ckeditor[_creole|_bbcode]#onEditorCreated` has been added to provide the
possibility of injecting JavaScript, when needed.

#### Why was this change made? [](id=why-was-this-change-made-53)

This change is part of a greater effort to provide mechanisms to extend and
configure any editor in Liferay Portal in a coherent and extensible way.

---------------------------------------

### Renamed ActionCommand Classes Used in the MVCPortlet Framework [](id=renamed-actioncommand-classes-used-in-the-mvcportlet-framework)
- **Date:** 2015-Jun-16
- **JIRA Ticket:** LPS-56372

#### What changed? [](id=what-changed-54a)

The classes located in the `com.liferay.portal.kernel.portlet.bridges.mvc`
package have been renamed to include the *MVC* prefix.

Old Classes:

- `BaseActionCommand`
- `BaseTransactionalActionCommand`
- `ActionCommand`
- `ActionCommandCache`

New Classes:

- `BaseMVCActionCommand`
- `BaseMVCTransactionalActionCommand`
- `MVCActionCommand`
- `MVCActionCommandCache`

Also, the property `action.command.name` has been renamed to `mvc.command.name`.
The code snippet below shows the new property in its context.

    @Component(
        immediate = true,
        property = {
                "javax.portlet.name=" + InvitationPortletKeys.INVITATION,
                "mvc.command.name=view"
        },
        service = MVCActionCommand.class
    )

#### Who is affected? [](id=who-is-affected-54a)

This affects any Java code calling the `ActionCommand` classes used in the
`MVCPortlet` framework.

#### How should I update my code? [](id=how-should-i-update-my-code-54a)

You should update the old `ActionCommand` class names with the new *MVC* prefix.

#### Why was this change made? [](id=why-was-this-change-made-54a)

This change adds consistency to the MVC framework, and makes it self-explanatory
what classes should be used for the MVC portlet.

---------------------------------------

### Extended MVC Framework to Use Same Key for Registering ActionURL and ResourceURL [](id=extended-mvc-framework-to-use-same-key-for-registering-actionurl-and-resour)
- **Date:** 2015-Jun-16
- **JIRA Ticket:** LPS-56372

#### What changed? [](id=what-changed-55a)

Previously, a single `ActionCommand` was valid for both `ActionURL` and
`ResourceURL`. Now you must distinguish both an `ActionURL` and `ResourceURL` as
different actions, which means you can register both with the same key.

#### Who is affected? [](id=who-is-affected-55a)

This affects developers that were using the `ActionCommand` for `actionURL`s and
`resourceURL`s.

#### How should I update my code? [](id=how-should-i-update-my-code-55a)

You should replace the `ActionCommand`s used for `actionURL`s and `resourceURL`s
to use `MVCActionCommand` and `MVCResourceCommand`, respectively. For example,
for the new `MVCResourceCommand`, you'll need to use the `resourceID` of the
`resourceURL` instead of using `ActionRequest.ACTION_NAME`.

Old Code:

    <liferay-portlet:resourceURL copyCurrentRenderParameters="<%= false %>" var="exportRecordSetURL">
        <portlet:param name="<%= ActionRequest.ACTION_NAME %>" value="exportRecordSet" />
        <portlet:param name="recordSetId" value="<%= String.valueOf(recordSet.getRecordSetId()) %>" />
    </liferay-portlet:resourceURL>

New Code:

    <liferay-portlet:resourceURL copyCurrentRenderParameters="<%= false %>" id="exportRecordSet" var="exportRecordSetURL">
        <portlet:param name="recordSetId" value="<%= String.valueOf(recordSet.getRecordSetId()) %>" />
    </liferay-portlet:resourceURL>

#### Why was this change made? [](id=why-was-this-change-made-55a)

This change was made to extend the MVC framework to have better support for
`actionURL`s and `resourceURL`s.

---------------------------------------

### Removed the liferay-ui:journal-article Tag [](id=removed-the-liferay-uijournal-article-tag)
- **Date:** 2015-Jun-29
- **JIRA Ticket:** LPS-56383

#### What changed? [](id=what-changed-54)

The `liferay-ui:journal-article` tag has been removed.

#### Who is affected? [](id=who-is-affected-54)

This affects developers using the `liferay-ui:journal-article` tag.

#### How should I update my code? [](id=how-should-i-update-my-code-54)

You should use the `liferay-ui:asset-display` tag instead.

**Example**

Old code:

    <liferay-ui:journal-article
        articleId="<%= article.getArticleId() %>"
    />

New code:

    <liferay-ui:asset-display
        className="<%= JournalArticleResource.class.getName() %>"
        template="<%= article.getResourcePrimKey() %>"
    />

#### Why was this change made? [](id=why-was-this-change-made-54)

The `liferay-ui:asset-display` is a generic way to display any type of asset.
Therefore, the `liferay-ui:journal-article` tag is no longer necessary.

---------------------------------------

### Changed Java Package Names for Portlets Extracted as Modules [](id=changed-java-package-names-for-portlets-extracted-as-modules)
- **Date:** 2015-Jun-29
- **JIRA Ticket:** LPS-56383 and others

#### What changed? [](id=what-changed-55)

The Java package names changed for portlets that were extracted as OSGi modules
in 7.0. Here is the complete list:

- `com.liferay.portlet.bookmarks` &rarr; `com.liferay.bookmarks`
- `com.liferay.portlet.dynamicdatalists` &rarr; `com.liferay.dynamicdatalists`
- `com.liferay.portlet.journal` &rarr; `com.liferay.journal`
- `com.liferay.portlet.polls` &rarr; `com.liferay.polls`
- `com.liferay.portlet.wiki` &rarr; `com.liferay.wiki`

#### Who is affected? [](id=who-is-affected-55)

This affects developers using the portlets API from their own plugins.

#### How should I update my code? [](id=how-should-i-update-my-code-55)

Update the package imports to use the new package names. Any literal usage of
the portlet `className` should also be updated.

#### Why was this change made? [](id=why-was-this-change-made-55)

Package names have been adapted to the new condition of Liferay portlets as
OSGi services.

---------------------------------------

### Removed the DLFileEntryTypes_DDMStructures Mapping Table [](id=removed-the-dlfileentrytypesddmstructures-mapping-table)
- **Date:** 2015-Jul-01
- **JIRA Ticket:** LPS-56660

#### What changed? [](id=what-changed-56)

The `DLFileEntryTypes_DDMStructures` mapping table is no longer available.

#### Who is affected? [](id=who-is-affected-56)

This affects developers using the Document Library File Entry Type Local Service
API.

#### How should I update my code? [](id=how-should-i-update-my-code-56)

Update the calls to `addDDMStructureLinks`, `deleteDDMStructureLinks`, and
`updateDDMStructureLinks` if you want to add, delete, or update references
between `DLFileEntryType` and `DDMStructures`.

#### Why was this change made? [](id=why-was-this-change-made-56)

This change was made to reduce the coupling between the two applications.

---------------------------------------

### Removed render Method from AssetRenderer API and WorkflowHandler API [](id=removed-render-method-from-assetrenderer-api-and-workflowhandler-api)
- **Date:** 2015-Jul-03
- **JIRA Ticket:** LPS-56705

#### What changed? [](id=what-changed-57)

The method `render` has been removed from the interfaces `AssetRenderer` and
`WorkflowHandler`.

#### Who is affected? [](id=who-is-affected-57)

This affects any Java code calling the method `render` on an `AssetRenderer` or
`WorkflowHandler` class, or Java classes overriding the `render` method of these
classes.

#### How should I update my code? [](id=how-should-i-update-my-code-57)

The method `render` was used to return the path of a JSP, including the
configuration of a portlet. That method is now available for the same
AssetRender API extending the `BaseJSPAssetRenderer` class, and is called
`getJspPath`.

If any logic was added to override the `render` method, it can now be added in
the `include` method.

#### Why was this change made? [](id=why-was-this-change-made-57)

This change was part of needed modifications to support adding asset renderers
and workflow handlers for portlets based on other technology different than JSP
(e.g., FreeMarker). The method `include` can now be used to create asset
renderers or workflow handlers with UIs written in FreeMarker or any other
framework.

---------------------------------------

### Renamed ADMIN_INSTANCE to PORTAL_INSTANCES in PortletKeys [](id=renamed-admininstance-to-portalinstances-in-portletkeys)
- **Date:** 2015-Jul-08
- **JIRA Ticket:** LPS-56867

#### What changed? [](id=what-changed-58)

The constant `PortletKeys.ADMIN_INSTANCE` has been renamed as
`PortletKeys.PORTAL_INSTANCES`.

#### Who is affected? [](id=who-is-affected-58)

This affects developers using the old constant in their code; for example,
creating a direct link to it. This is not common and usually not a good
practice, so this should not affect many people.

#### How should I update my code? [](id=how-should-i-update-my-code-58)

You should rename the constant `ADMIN_INSTANCE` to `PORTAL_INSTANCES`
everywhere it is used.

#### Why was this change made? [](id=why-was-this-change-made-58)

This change was part of needed modifications to extract the Portal Instances
portlet from the Admin portlet. The constant's old name was not accurate, since
it originated from the old Admin portlet. Since the Portal Instances portlet
is now extracted to its own module, the old name no longer resembles its usage.

---------------------------------------

### Removed Support for filterFindBy Generation or InlinePermissionUtil Usage for Tables When the Primary Key Type Is Not long [](id=removed-support-for-filterfindby-generation-or-inlinepermissionutil-usage-f)
- **Date:** 2015-Jul-21
- **JIRA Ticket:** LPS-54590

#### What changed? [](id=what-changed-59)

ServiceBuilder and inline permission filter support has been removed for
non-`long` primary key types.

#### Who is affected? [](id=who-is-affected-59)

This affects code that is using `int`, `float`, `double`, `boolean`, or `short`
type primary keys in the `service.xml` with inline permissions.

#### How should I update my code? [](id=how-should-i-update-my-code-59)

You should change the primary key type to `long`.

#### Why was this change made? [](id=why-was-this-change-made-59)

Inline permissioning was using the `join` method between two different data
types and that caused significant performance degradation with `filterFindBy`
queries.

---------------------------------------

### Removed Vaadin 6 from Liferay Core [](id=removed-vaadin-6-from-liferay-core)
- **Date:** 2015-Jul-31
- **JIRA Ticket:** LPS-57525

#### What changed? [](id=what-changed-60)

The bundled Vaadin 6.x JAR file has been removed from portal core.

#### Who is affected? [](id=who-is-affected-60)

This affects developers who are creating Vaadin portlet applications in Liferay
Portal.

#### How should I update my code? [](id=how-should-i-update-my-code-60)

You should upgrade to Vaadin 7, bundle your `vaadin.jar` with your plugin, or
deploy Vaadin libraries to Liferay's OSGi container.

#### Why was this change made? [](id=why-was-this-change-made-60)

Vaadin 6.x is outdated and there are no plans for any new projects to be
created with it. Therefore, developers should begin using Vaadin 7.x.

---------------------------------------

### Replaced the Navigation Menu Portlet's Display Styles with ADTs [](id=replaced-the-navigation-menu-portlets-display-styles-with-adts)
- **Date:** 2015-Jul-31
- **JIRA Ticket:** LPS-27113

#### What changed? [](id=what-changed-61)

The custom display styles of the navigation tag added using JSPs no longer work.
They have been replaced by Application Display Templates (ADT).

#### Who is affected? [](id=who-is-affected-61)

This affects developers that use portlet properties with the following prefix:

    navigation.display.style

This also affects developers that use the following attribute in the navigation
tag:

    displayStyleDefinition

#### How should I update my code? [](id=how-should-i-update-my-code-61)

To style the Navigation portlet, you should use ADTs instead of using custom
styles in your JSPs. ADTs can be created from the UI of the portal by navigating
to *Site Settings* &rarr; *Application Display Templates*. ADTs can also be
created programatically.

Developers should use the `ddmTemplateGroupId` and `ddmTemplateKey` attributes
of the navigation tag to set the ADT that defines the style of the navigation.

#### Why was this change made? [](id=why-was-this-change-made-61)

ADTs allow you to change an application's look and feel without changing its JSP
code.

---------------------------------------

### Renamed URI Attribute Used to Generate AUI Tag Library [](id=renamed-uri-attribute-used-to-generate-aui-tag-library)
- **Date:** 2015-Aug-12
- **JIRA Ticket:** LPS-57809

#### What changed? [](id=what-changed-62)

The URI attribute used to identify the AUI taglib has been renamed.

#### Who is affected? [](id=who-is-affected-62)

This affects developers that use the URI `http://alloy.liferay.com/tld/aui` in
their JSPs, XMLs, etc.

#### How should I update my code? [](id=how-should-i-update-my-code-62)

You should use the new AUI URI declaration:

Old:

    http://alloy.liferay.com/tld/aui

New:

    http://liferay.com/tld/aui

#### Why was this change made? [](id=why-was-this-change-made-62)

To stay consistent with other taglibs provided by Liferay, the AUI `.tld` file
was modified to start with the prefix `liferay-`. Due to this change, the XML
files used to automatically generate the AUI taglib were modified, changing the
AUI URI declaration.

---------------------------------------

### Removed Support for runtime-portlet Tag in Body of Web Content Articles [](id=removed-support-for-runtime-portlet-tag-in-body-of-web-content-articles)
- **Date:** 2015-Sep-17
- **JIRA Ticket:** LPS-58736

#### What changed? [](id=what-changed-63)

The tag `runtime-portlet` is no longer replaced by a portlet if it is found in
the body of a web content article.

#### Who is affected? [](id=who-is-affected-63)

This affects any web content in the database (`JournalArticle` table) that uses
this tag.

#### How should I update my code? [](id=how-should-i-update-my-code-63)

Embedding another portlet is only supported from a template. You should embed
the portlet by passing its name in a call to `theme.runtime` or using the right
tag in FreeMarker.

**Example**

In Velocity:

    $theme.runtime("145")

In FreeMarker:

    <#assign liferay_portlet = PortalJspTagLibs["/WEB-INF/tld/liferay-portlet-ext.tld"] />

    <@liferay_portlet["runtime"] portletName="145" />

#### Why was this change made? [](id=why-was-this-change-made-63)

This change improves the performance of web content articles while enforcing a
single way to embed portlets into the page for better testing.

---------------------------------------

### Removed the liferay-ui:control-panel-site-selector Tag [](id=removed-the-liferay-uicontrol-panel-site-selector-tag)
- **Date:** 2015-Sep-23
- **JIRA Ticket:** LPS-58210

#### What changed? [](id=what-changed-64)

The tag `liferay-ui:control-panel-site-selector` has been deleted.

#### Who is affected? [](id=who-is-affected-64)

This affects developers who use this tag in their code.

#### How should I update my code? [](id=how-should-i-update-my-code-64)

You should consider using the tag `liferay-ui:my-sites`, or create your own
markup using the `GroupService` API.

#### Why was this change made? [](id=why-was-this-change-made-64)

This tag is no longer used and will no longer be maintained properly.

---------------------------------------

### Removed Methods Related to Control Panel in PortalUtil [](id=removed-methods-related-to-control-panel-in-portalutil)
- **Date:** 2015-Sep-23
- **JIRA Ticket:** LPS-58210

#### What changed? [](id=what-changed-65)

The following methods have been deleted:

- `getControlPanelCategoriesMap`
- `getControlPanelCategory`
- `getControlPanelPortlets`
- `getFirstMyAccountPortlet`
- `getFirstSiteAdministrationPortlet`
- `getSiteAdministrationCategoriesMap`
- `getSiteAdministrationURL`
- `isCompanyControlPanelVisible`

#### Who is affected? [](id=who-is-affected-65)

This affects developers that use any of the methods listed above.

#### How should I update my code? [](id=how-should-i-update-my-code-65)

In order to work with applications displayed in the Product Menu, developers
should call the `PanelCategoryRegistry` and `PanelAppRegistry` classes located
in the `application-list-api` module. These classes allow developers to interact
with categories and applications in the Control Panel.

#### Why was this change made? [](id=why-was-this-change-made-65)

These methods are no longer used and they will not work properly since they
cannot call the `application-list-api` from the portal context.

---------------------------------------

### Removed ThemeDisplay Methods Related to Control Panel and Site Administration [](id=removed-themedisplay-methods-related-to-control-panel-and-site-administrati)
- **Date:** 2015-Sep-23
- **JIRA Ticket:** LPS-58210

#### What changed? [](id=what-changed-66)

The following methods have been deleted:

- `getControlPanelCategory`
- `getURLSiteAdministration`

#### Who is affected? [](id=who-is-affected-66)

This affects developers that use either of the methods listed above.

#### How should I update my code? [](id=how-should-i-update-my-code-66)

Site Administration is not a site per se; some applications are displayed in
that context. To create a link to an application that is displayed in Site
Administration, developers should use the method
`PortalUtil.getControlPanelURL`. In order to obtain the first application
displayed in a section of the Product Menu, developers should use the
`application-list-api` module to call the `PanelCategoryRegistry` and
`PanelAppRegistry` classes.

#### Why was this change made? [](id=why-was-this-change-made-66)

These methods are no longer used and they will not work properly since they
cannot call the `application-list-api` from the portal context.

---------------------------------------

### Removed Control Panel from List of Sites Returned by Methods Group.getUserSitesGroups and User.getMySiteGroups [](id=removed-control-panel-from-list-of-sites-returned-by-methods-group-getusers)
- **Date:** 2015-Sep-23
- **JIRA Ticket:** LPS-58862

#### What changed? [](id=what-changed-67)

The following methods had a boolean parameter to determine whether to include
the Control Panel group:

- `Group.getUserSitesGroups`
- `User.getMySiteGroups`

This boolean parameter should no longer be used.

#### Who is affected? [](id=who-is-affected-67)

This affects developers that use either of the methods listed above passing the
`includeControlPanel` parameter as `true`.

#### How should I update my code? [](id=how-should-i-update-my-code-67)

If you don't need the Control Panel, remove the `false` parameter. If you still
want to obtain a link to the Control Panel, you should do it in a different way.

The Control Panel is not a site per se; some applications are displayed in that
context. To create a link to an application that is displayed in the Control
Panel, developers should use the method `PortalUtil.getControlPanelURL`. In
order to obtain the first application displayed in a section of the Product
Menu, developers should use the `application-list-api` module to call the
`PanelCategoryRegistry` and `PanelAppRegistry` classes.

#### Why was this change made? [](id=why-was-this-change-made-67)

The Control Panel is no longer a site per se, but just a context in which some
applications are displayed. This concept conflicts with the idea of returning a
site called Control Panel in the Sites API.

---------------------------------------

### Changed Exception Thrown by Documents and Media Services When Duplicate Files are Found [](id=changed-exception-thrown-by-documents-and-media-services-when-duplicate-fil)
- **Date:** 2015-Sep-24
- **JIRA Ticket:** LPS-53819

#### What changed? [](id=what-changed-68a)

When a duplicate file entry is found by Documents and Media (D&M) services, a
`DuplicateFileEntryException` will be thrown. Previously, the exception
`DuplicateFileException` was used.

The `DuplicateFileException` is now raised only by `Store`
implementations.

#### Who is affected? [](id=who-is-affected-68a)

Any caller of the `addFileEntry` methods in `DLApp` and `DLFileEntry`
local and remote services is affected.

#### How should I update my code? [](id=how-should-i-update-my-code-68a)

Change the exception type from `DuplicateFileException` to
`DuplicateFileEntryException` in `try-catch` blocks surrounding calls
to D&M services.

#### Why was this change made? [](id=why-was-this-change-made-68a)

The `DuplicateFileException` exception was used in two different
contexts:
- When creating a new file through D&M and a row in the database
already existed for a file entry with the same title.
- When the stores tried to save a file and the underlying storage unit
(a file in the case of `FileSystemStore`) already existed.

This made it impossible to detect and recover from store corruption
issues, as they were undifferentiable from other errors.

---------------------------------------

### Removed All References to Windows Live Messenger [](id=removed-all-references-to-windows-live-messenger)
- **Date:** 2015-Oct-15
- **JIRA Ticket:** LPS-30883

#### What changed? [](id=what-changed-68)

All references to the `msnSn` column in the Contacts table have been removed
from portal. All references to Windows Live Messenger have been removed from
properties, tests, classes, and the frontend. Also, the `getMsnSn` and
`setMsnSn` methods have been removed from the `Contact` and `LDAPUser` models.

The following classes have been removed:

- `MSNConnector`
- `MSNMessageAdapter`

The following constants have been removed:

- `CalEventConstants.REMIND_BY_MSN`
- `ContactConverterKeys.MSN_SN`
- `PropsKeys.MSN_LOGIN`
- `PropsKeys.MSN_PASSWORD`

The following methods have been removed:

- `Contact.getMsnSn`
- `Contact.setMsnSn`
- `LDAPUser.getMsnSn`
- `LDAPUser.setMsnSn`

The following methods have been changed:

- `AdminUtil.updateUser`
- `ContactLocalServiceUtil.addContact`
- `ContactLocalServiceUtil.updateContact`
- `UserLocalServiceUtil.addContact`
- `UserLocalServiceUtil.updateContact`
- `UserLocalServiceUtil.updateUser`
- `UserServiceUtil.updateUser`

#### Who is affected? [](id=who-is-affected-68)

This affects developers who use any of the classes, constants, or methods listed
above.

#### How should I update my code? [](id=how-should-i-update-my-code-68)

When updating or adding a user or contact using one of the changed methods
above, remove the `msnSn` argument from the method call. If you are using one of
the removed items above, you should remove all references to them from your code
and look for alternatives, if necessary. Lastly, remove any references to the
`msnSN` column in the Contacts table from your SQL queries.

#### Why was this change made? [](id=why-was-this-change-made-68)

Since Microsoft dropped support for Windows Live Messenger, Liferay will no
longer continue to support it.

---------------------------------------

### Removed Support for AIM, ICQ, MySpace, and Yahoo Messenger [](id=removed-support-for-aim-icq-myspace-and-yahoo-messenger)
- **Date:** 2015-Oct-22
- **JIRA Ticket:** LPS-59716

#### What changed? [](id=what-changed-69)

Liferay no longer supports integration with MySpace and AIM, ICQ, and Yahoo
Messenger instant messaging services. The corresponding `aimSn`, `icqSn`,
`mySpaceSn`, and `ymSn` columns have been removed from the `Contacts` table.

The following classes have been removed:

- `AIMConnector`
- `ICQConnector`
- `YMConnector`

The following constants have been removed:

- `CalEventConstants.REMIND_BY_AIM`
- `CalEventConstants.REMIND_BY_ICQ`
- `CalEventConstants.REMIND_BY_YM`
- `ContactConverterKeys.AIM_SN`
- `ContactConverterKeys.ICQ_SN`
- `ContactConverterKeys.MYSPACE_SN`
- `ContactConverterKeys.YM_SN`
- `PropsKeys.AIM_LOGIN`
- `PropsKeys.AIM_PASSWORD`
- `PropsKeys.ICQ_JAR`
- `PropsKeys.ICQ_LOGIN`
- `PropsKeys.ICQ_PASSWORD`
- `PropsKeys.YM_LOGIN`
- `PropsKeys.YM_PASSWORD`

The following methods have been removed:

- `getAimSn`
- `getIcqSn`
- `getMySpaceSn`
- `getYmSn`
- `setAimSn`
- `setIcqSn`
- `setMySpaceSn`
- `setYmSn`

The following methods have been changed:

- `updateUser`
- `addContact`

The following portal properties have been removed:

- `aim.login`
- `aim.password`
- `icq.jar`
- `icq.login`
- `icq.password`
- `ym.login`
- `ym.password`

#### Who is affected? [](id=who-is-affected-69)

This affects developers who use any of the classes, constants, methods, or
properties listed above.

#### How should I update my code? [](id=how-should-i-update-my-code-69)

When updating or adding a user or contact using one of the changed methods
above, remove the `aimSn`, `icqSn`, `mySpaceSn`, and `ymSn` arguments from the
method call. If you are using one of the removed items above, you should remove
all references to them from your code and look for alternatives, if necessary.
Lastly, remove from your SQL queries any references to former `Contacts` table
columns `aimSn`, `icqSn`, `mySpaceSn`, and `ymSn`.

Also, a reference to any one of the removed portal properties above no longer
returns a value.

#### Why was this change made? [](id=why-was-this-change-made-69)

The services removed in this change are no longer popular enough to merit
continued support.

---------------------------------------

### Removed All Methods from SchedulerEngineHelper that Explicitly Schedule Jobs Using SchedulerEntry or Specify MessageListener Class Names [](id=removed-all-methods-from-schedulerenginehelper-that-explicitly-schedule-job)
- **Date:** 2015-Oct-29
- **JIRA Ticket:** LPS-59681

#### What changed? [](id=what-changed-70)

The following methods were removed from `SchedulerEngine`:

- `SchedulerEngineHelper.addJob(Trigger, StorageType, String, String, Message, String, String, int)`
- `SchedulerEngineHelper.addJob(Trigger, StorageType, String, String, Object, String, String, int)`
- `SchedulerEngineHelper.schedule(SchedulerEntry, StorageType, String, int)`

#### Who is affected? [](id=who-is-affected-70)

This affects developers that use the above methods to schedule jobs into the
`SchedulerEngine`.

#### How should I update my code? [](id=how-should-i-update-my-code-70)

You should update your code to call one of these methods:

- `SchedulerEngineHelper.schedule(Trigger, StorageType, String, String, Message, int)`
- `SchedulerEngineHelper.schedule(Trigger, StorageType, String, String, Object, int)`

Instead of simply providing the class name of your scheduled job listener, you
should follow these steps:

1.  Instantiate your MessageListener.

2.  Call `SchedulerEngineHelper.register(MessageListener, SchedulerEntry)` to
    register your `SchedulerEventMessageListener`.


#### Why was this change made? [](id=why-was-this-change-made-70)

The deleted methods provided facilities that aren't compatible with using
declarative services in an OSGI container. The new approach allows for proper
injection of dependencies into scheduled event message listeners.

---------------------------------------

### Removed the asset.publisher.query.form.configuration Property [](id=removed-the-asset-publisher-query-form-configuration-property)
- **Date:** 2015-Nov-03
- **JIRA Ticket:** LPS-60119

#### What changed? [](id=what-changed-71)

The `asset.publisher.query.form.configuration` property has been removed
from `portal.properties`.

#### Who is affected? [](id=who-is-affected-71)

This affects any hook that uses the `asset.publisher.query.form.configuration`
property.

#### How should I update my code? [](id=how-should-i-update-my-code-71)

If you are using this property to generate the UI for an Asset Entry Query
Processor, your Asset Entry Query Processor must now implement the `include`
method to generate the UI.

#### Why was this change made? [](id=why-was-this-change-made-71)

This change was made as a part of the ongoing strategy to modularize Liferay
Portal.

---------------------------------------

### Removed Hover and Alternate Style Features of Search Container Tag [](id=removed-hover-and-alternate-style-features-of-search-container-tag)
- **Date:** 2015-Nov-03
- **JIRA Ticket:** LPS-58854

#### What changed? [](id=what-changed-72)

The following attributes and methods have been removed:

- The attribute `hover` of the `liferay-ui:search-container` tag.
- The method `isHover()` of the `SearchContainerTag` class.
- The attributes `classNameHover`, `hover`, `rowClassNameAlternate`,
`rowClassNameAlternateHover`, `rowClassNameBody`, `rowClassNameBodyHover` of the
`liferay-search-container` JavaScript module.

#### Who is affected? [](id=who-is-affected-72)

This affects developers that use the `hover` attribute of the
`liferay-ui:search-container` tag.

#### How should I update my code? [](id=how-should-i-update-my-code-72)

You should update your code changing the CSS selector that defines how rows look
on hover to use the `:hover` and `:nth-of-type` CSS pseudo selectors instead.

#### Why was this change made? [](id=why-was-this-change-made-72)

Browsers support better ways to style content on hover in a way that doesn't
penalize performance. Therefore, this change was made to increase the
performance of hovering over content in Liferay.

---------------------------------------

### Removed AppViewMove and AppViewSelect JavaScript Modules [](id=removed-appviewmove-and-appviewselect-javascript-modules)
- **Date:** 2015-Nov-03
- **JIRA Ticket:** LPS-58854

#### What changed? [](id=what-changed-73)

The JavaScript modules `AppViewMove` and `AppViewSelect` have been removed.

#### Who is affected? [](id=who-is-affected-73)

This affects developers that use these modules to configure *select* and *move*
actions inside their applications.

#### How should I update my code? [](id=how-should-i-update-my-code-73)

If you are using any of these modules, you can make use of the following
`SearchContainer` APIs:

- Listen to the `rowToggled` event of the search container to be notified about
changes to the search container state.
- Configure your search container *move* options creating a `RowMover` and
define the allowed *move* targets and associated actions.
- Use the `registerAction` method of the search container to execute your *move*
logic when the user completes a *move* action.

#### Why was this change made? [](id=why-was-this-change-made-73)

The removed JavaScript modules contained too much logic and were difficult to
decipher. It was also difficult to add this to an existing app. With this
change, every app using a search container can use this functionality much
easier.

---------------------------------------

### Removed the mergeLayoutTags Preference from Asset Publisher [](id=removed-the-mergelayouttags-preference-from-asset-publisher)
- **Date:** 2015-Nov-20
- **JIRA Ticket:** LPS-60677

#### What changed? [](id=what-changed-74)

The `mergeLayoutTags` preference has been removed from the Asset Publisher.

#### Who is affected? [](id=who-is-affected-74)

This affects any Asset Publisher portlet that uses this preference.

#### How should I update my code? [](id=how-should-i-update-my-code-74)

There is nothing to update since this functionality is no longer used.

#### Why was this change made? [](id=why-was-this-change-made-74)

In previous versions of Liferay, some applications such as Blogs and Wiki shared
the tags of their entries within the page. The Asset Publisher was able to use
them to show other assets with the same tags. This functionality has changed, so
the preference is no longer used.

---------------------------------------

### Removed Software Catalog Portlet and Services
- **Date:** 2015-Nov-21
- **JIRA Ticket:** LPS-60705

#### What changed?

The Software Catalog portlet and its associated services are no longer part
of Liferay's source code or binaries.

#### Who is affected?

This affects portals which were making use of the Software Catalog portlet to
manage a catalog of their software. Developers who were making use of the
software catalog services from their custom code are also affected.

#### How should I update my code?

There is no direct replacement for invocations to the Software Catalog services.
In cases where it is really needed, it is possible to obtain the code from a
previous release and include it in the custom product (subject to licensing).

#### Why was this change made?

The Software Catalog was developed to implement the very first versions of what
later become Liferay's Marketplace. It was later replaced and has not been used
by Liferay since then. It has also been used minimally outside of Liferay. The
decision was made to remove it so Liferay could be more lightweight and free
time to focus on other areas of the product that add more value.

---------------------------------------

### Removed the getPageOrderByComparator Method from WikiUtil
- **Date:** 2015-Dec-01
- **JIRA Ticket:** LPS-60843

#### What changed?

The `getPageOrderByComparator` method has been removed from `WikiUtil`.

#### Who is affected?

This affects developers that use this method in their code.

#### How should I update my code?

You should update your code to invoke
`WikiPortletUtil.getPageOrderByComparator(String, String)`.

#### Why was this change made?

As part of the modularization efforts it has been considered that that this
logic belongs to wiki-web module.

---------------------------------------

### Custom AUI Validators Are No Longer Implicitly Required
- **Date:** 2015-Dec-02
- **JIRA Ticket:** LPS-60995

#### What changed?

The AUI Validator tag no longer forces custom validators (e.g., `name="custom"`)
to be required, and are now optional by default.

#### Who is affected?

This affects developers using custom validators, especially ones who relied on
the field being implicitly required via the custom validator.

#### How should I update my code?

There are several cases where you should update your code to compensate for this
change. First, blank value checking is no longer necessary, so places where
blank values are checked should be updated.

Old Code:

    return !val || val != A.one('#<portlet:namespace />publicVirtualHost').val();

New Code:

    return val != A.one('#<portlet:namespace />publicVirtualHost').val();

Also, instead of using custom validators to determine if a field is required,
you should now use a conditional `required` validator.

Old Code:

    <aui:validator errorMessage="you-must-specify-a-file-or-a-title" name="custom">
        function(val, fieldNode, ruleValue) {
            return !!val || !!A.one('#<portlet:namespace />file').val();
    }

New Code:

    <aui:validator errorMessage="you-must-specify-a-file-or-a-title" name="required">
        function(fieldNode) {
            return !A.one('#<portlet:namespace />file').val();
    }

Lastly, custom validators that assumed validation would always run must now
explicitly pass the `required` validator. This is done by passing in the
`<aui:validator name="required" />` element. The `<aui:input>` tag listed below
is an example of how to explicity pass the `required` validator:

    <aui:input name="vowelsOnly">
        <aui:validator errorMessage="must-contain-only-the-following-characters" name="custom">
            function(val, fieldNode, ruleValue) {
                var allowedCharacters = 'aeiouy';
                var regex = new RegExp('[^' + allowedCharacters + ']');

                return !regex.test(val);
            }
        </aui:validator>
        <aui:validator name="required" />
    </aui:input>

#### Why was this change made?

A custom validator caused the field to be implicitly required. This meant that
all validators for the field would be evaluated. This created a condition where
you could not combine custom validators with another validator for an optional
field.

For example, imagine an optional field which has an email validator, plus a
custom validator which checks for email addresses within a specific domain
(e.g., `example.com`). There was no way for this optional field to pass
validation. Even if you handled blank values in your custom validator, that
blank value would fail the email validator.

This change requires most custom validators to be refactored, but allows greater
flexibility for all developers.

---------------------------------------

### Moved Recycle Bin logic into a new `DLTrashService` interface
- **Date:** 2015-Dec-02
- **JIRA Ticket:** LPS-60810

#### What changed?

All Recycle Bin logic in Documents and Media services was moved from
`DLAppService` into the new `DLTrashService` service interface. All moved
methods have the same name and signatures.

#### Who is affected?

This affects any local or remote caller of `DLAppService`.

#### How should I update my code?

As all methods have been simply moved into the new service, calling the
equivalent method on `DLTrashService` suffices.

#### Why was this change made?

Documents and Media services have complex interdependencies that result in
circular dependencies. Until now, `DLAppService` was responsible for exposing
the Recycle Bin logic, delegating it to other components. The problem was, the
components depended on `DLAppService` to implement their logic. Extracting the
services from `DLAppService` was the only sensible solution to this circularity.

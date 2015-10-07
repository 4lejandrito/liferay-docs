# Web Content Display Screenlet for iOS [](id=webcontentdisplayscreenlet-for-ios)

<iframe width="560" height="315" src="https://www.youtube.com/embed/ODfb_4igvCs" frameborder="0" allowfullscreen></iframe>

## Requirements [](id=requirements)

- XCode 6.4.x
- iOS 8 SDK
- Liferay Portal 6.2 CE or EE
- Liferay Screens Compatibility Plugin (
  [CE](http://www.liferay.com/marketplace/-/mp/application/54365664) or 
  [EE](http://www.liferay.com/marketplace/-/mp/application/54369726), 
  depending on your portal edition). 

## Compatibility [](id=compatibility)

- iOS 7 and above

## Features [](id=features)

The `WebContentDisplayScreenlet` shows web content elements in your app, 
rendering the inner HTML of the web content. The Screenlet also supports i18n, 
rendering contents differently depending on the device's current locale. 

## Module [](id=module)

- None

## Themes [](id=themes)

The Default Theme uses a standard `UIWebView` to render the HTML. Other Themes 
may use a different component, such as iOS 8's. 

![The `WebContentDisplayScreenlet` using the Default (`default`) Theme](../../images/screens-ios-webcontent.png)

## Portal Configuration [](id=portal-configuration)

For the `WebContentDisplayScreenlet` to function properly, there should be web 
content in the Liferay instance your app connects to. For more details on web 
content, please refer to the [Web Content Management](/portal/-/knowledge_base/6-2/web-content-management) 
section of the Liferay User Guide. 

## Offline [](id=offline)

This Screenlet supports offline mode so it can function without a network 
connection. 

| Policy | What happens | When to use |
|--------|--------------|-------------|
| `remote-only` | The Screenlet loads the content from the portal. If a connection issue occurs, the Screenlet uses the delegate to notify the developer about the error. If the Screenlet successfully loads the content, it stores the data in the local cache for later use. | Use this policy when you always need to show updated content, and show nothing when there's no connection. |
| `cache-only` | The Screenlet loads the content from the local cache. If the content isn't there, the Screenlet uses the delegate to notify the developer about the error. | Use this policy when you always need to show local content, without retrieving remote content under any circumstance. |
| `remote-first` | The Screenlet loads the content from the portal. If this succeeds, the Screenlet shows the content to the user and stores it in the local cache for later use. If a connection issue occurs, the Screenlet retrieves the content from the local cache. If the content doesn't exist there, the Screenlet uses the delegate to notify the developer about the error. | Use this policy to show the most recent version of the content when connected, but show a possibly outdated version when there's no connection. |
| `cache-first` | The Screenlet loads the content from the local cache. If the content isn't there, the Screenlet requests it from the portal and notifies the developer about any errors that occur (including connectivity errors). | Use this policy to save bandwidth and loading time in case you have local (but probably outdated) content. |

## Attributes [](id=attributes)

| Attribute | Data type | Explanation |
|-----------|-----------|-------------| 
| `groupId` | `number` | The site (group) identifier where the asset is stored. If this value is `0`, the `groupId` specified in `LiferayServerContext` is used. |
| `articleId` | `string` | The identifier of the web content to display. You can find the identifier by clicking *Edit* on the web content in the portal. |

## Methods [](id=methods)

| Method | Return | Explanation |
|-----------|-----------|-------------| 
|  `loadWebContent()` | `boolean` | Starts the request to load the web content. The HTML is rendered when the response is received. Returns `true` if the request is sent. |

## Delegate [](id=delegate)

The `WebContentDisplayScreenlet` delegates some events to an object that 
conforms to the `WebContentDisplayScreenletDelegate` protocol. This protocol 
lets you implement the following methods:

- `- screenlet:onWebContentResponse:`: Called when the web content's HTML is 
  received. 

- `- screenlet:onWebContentDisplayError:`: Called when an error occurs in the 
  process. The `NSError` object describes the error. 

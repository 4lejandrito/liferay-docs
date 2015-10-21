# Customizing Liferay Services

Blade Tools provides templates for creating modules for customizing Liferay
services. Service wrappers allow Liferay services to be easily customized. In
this tutorial, you'll learn how to create service wrapper modules. If you
haven't yet installed Blade Tools, please refer to the
[Introduction to Blade Tools](need link) tutorial.

Why might you need to customize Liferay services? Perhaps you've added a custom
field (`Expando` is the back-end term for a custom field) to Liferay's `User`
object and you want a value for the custom field to be saved whenever the
`addUser` or `updateUser` methods of Liferay's API are called. Or maybe you
simply want to add some additional logging functionality to some of Liferay's
APIs. Whatever your case may be, Liferay's service wrappers provide easy-to-use
extension points for customize Liferay's services.

To create a module that overrides one of Liferay's services, use the
`servicewrapper` project type. Use this command 

    blade create -p servicewrapper -c UserLocalServiceOverride -d . com.liferay.docs.serviceoverride com.liferay.portal.service.UserLocalServiceWrapper

to create a Gradle-based service wrapper module project named
`com.liferay.docs.serviceoverride` in your current directory. `-p
servicewrapper` specifies that you're creating a service wrapper project. `-c
UserLocalServiceOverride` specifies the name of the component class that you're
creating. `-d .` specifies that you're creating the project in your current
working directory. The `com.liferay.docs.serviceoverride` specifies both the
name of the project to be created and the package in which your component class
is to be created. The `com.liferay.portal.service.UserLocalServiceWrapper`
argument specifies the specific type of service wrapper that your component
class should extend. 

Here's the project structure that's created by the `create` command above:

- `com.liferay.docs.serviceoverride`
    - `src`
        - `main`
            - `java`
                - `com/liferay/docs/serviceoverride`
                    - `UserLocalServiceOverride.java`
    - `bnd.bnd`
    - `build.gradle`

Here's the `UserLocalServiceOverride` class that's generated:

    package com.liferay.docs.serviceoverride;

    import com.liferay.portal.service.UserLocalServiceWrapper;
    import com.liferay.portal.service.ServiceWrapper;
    import org.osgi.service.component.annotations.Component;

    @Component(
        immediate = true,
        property = {
        },
        service = ServiceWrapper.class
    )
    public class UserLocalServiceOverride extends UserLocalServiceWrapper {

        public UserLocalServiceOverride() {
            super(null);
        }

    }

Notice that you must specify the fully qualified class name of the specific
service wrapper class that you want to extend. The `service` argument was used
in full in this import statement:

    import com.liferay.portal.service.UserLocalServiceWrapper

This import statement, in turn, allowed the short form of the service wrapper
class name to be used in the class declaration of your component class:

    public class UserLocalServiceOverride extends UserLocalServiceWrapper`

The bottom line is that when using `blade create` to create a service wrapper
project, you must specify a fully qualified class name as the `service`
argument. (This is also true when using `blade create` to create a service
project.) For information about creating service projects, please see the
[Not yet written]() tutorial.

The generated `UserLocalServiceOverride` class does not actually customize any
Liferay service. Before you can test that your service wrapper module actually
works, you need to override at least one service method.

Open your `UserLocalServiceOverride` class and add the following methods:

    @Override
    public int authenticateByEmailAddress(long companyId, String emailAddress,
            String password, Map<String, String[]> headerMap,
            Map<String, String[]> parameterMap, Map<String, Object> resultsMap)
        throws PortalException {

        System.out.println(
            "Authenticating user by email address " + emailAddress);
        return super.authenticateByEmailAddress(companyId, emailAddress, password,
            headerMap, parameterMap, resultsMap);
    }

    @Override
    public User getUser(long userId) throws PortalException {
        System.out.println("Getting user by id " + userId);
        return super.getUser(userId);
    }

Each of these methods overrides a Liferay service method. These implementations
merely add a few print statements that are executed before the original service
implementations are invoked.

Now you're ready to build your project. Simply navigate to your project's root
directory and run `gradle build`. The JAR file representing your portlet module
is produced in your project's `build/libs` directory. If you followed the
example in this tutorial, it should be called
`com.liferay.docs.serviceoverride-1.0.jar`.

To deploy your project, run this command from your project's root directory:

    blade deploy build/libs/com.liferay.docs.serviceoverride-1.0.jar

Blade Tools detects your locally running Liferay instance and deploys the
specified module to Liferay's module framework. After running the `blade deploy
[path to JAR]` command, you should see a message like this:

    Installed or updated bundle 334

Use the Gogo shell to confirm that your module was installed: Open a terminal
and enter `telnet localhost 11311`. Then enter `lb` at the prompt. If your
module was installed, you'll see an entry like this:

    335|Active     |    1|com.liferay.docs.serviceoverride (1.0.0.201510192109)

Finally, log into your portal as an administrator. Navigate to the Users section
of the Control Panel. Confirm that your customizations of Liferay's user service
methods have taken effect by checking Liferay's log for the print statements
that you added. Congratulations! You've created and deployed a Liferay 7 service
wrapper module!

﻿<a name="HOLTop"></a>
# Service Bus Topics#
---

<a name="Overview"></a>
## Overview ##

// TODO: Review Overview

**Service Bus Topics** contains a brand-new set of cloud-based, message-oriented-middleware technologies including a fully-featured **Message Queue** with support for arbitrary content types, rich |message properties, correlation, reliable binary transfer, and grouping. Another important feature is **Service Bus Topics** which provide a set of publish-and-subscribe capabilities and are based on the same backend infrastructure as **Service Bus Queues**. A **Topic** consists of a sequential message store just like a **Queue**, but allows for many concurrent and durable **Subscriptions** that can independently yield copies of the published messages to consumers. Each **Subscription** can define a set of rules with simple expressions that specify which messages from the published sequence are selected into the Subscription.

<a name="Objectives"></a>
### Objectives ###

In this hands-on lab, you will learn how to:

- Create a Service Bus Namespace
- Create Topics and Subscriptions
- Use Subscription Filter Expressions
- Use Subscription Filter Actions

<a name="Prerequisites"></a>
### Prerequisites ###

You must have the following items to complete this lab:

- [Visual Studio Express 2013 for Web][1] or greater

- [Windows Azure Tools for Microsoft Visual Studio 2.2 (or later)][2]

- A Windows Azure subscription
	- Sign up for a [Free Trial](http://aka.ms/watk-freetrial)
	- If you are a Visual Studio Professional, Test Professional, Premium or Ultimate with MSDN or MSDN Platforms subscriber, activate your [MSDN benefit](http://aka.ms/watk-msdn) now to start development and test on Windows Azure.
	- [BizSpark](http://aka.ms/watk-bizspark) members automatically receive the Windows Azure benefit through their Visual Studio Ultimate with MSDN subscriptions.
	- Members of the [Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials program receive monthly credits of Windows Azure at no charge.

[1]: http://www.microsoft.com/visualstudio/
[2]: http://www.microsoft.com/windowsazure/sdk/

>**Note:** This lab was designed to use Windows 8 Operating System.

<a name="Setup"></a>
### Setup ###
In order to execute the exercises in this hands-on lab you need to set up your environment.

1. Open a Windows Explorer window and browse to the lab's **Source** folder.

1. Execute the **Setup.cmd** file with Administrator privileges to launch the setup process that will configure your environment and install the Visual Studio Code Snippets for this lab.

1. If the User Account Control dialog is shown, confirm the action to proceed.

 
> **Note:** Make sure you have checked all the dependencies for this lab before running the setup.

<a name="UsingCodeSnippets"></a>
### Using the Code Snippets ###

Throughout the lab document, you will be instructed to insert code blocks. For your convenience, most of that code is provided as Visual Studio Code Snippets, which you can use from within Visual Studio 2013 to avoid having to add it manually.

---

<a name="Exercises"></a>
## Exercises ##

This hands-on lab includes the following exercises:

1. [Creating a Topic and Adding Subscriptions](#Exercise1)
1. [Using a Subscription Rule Filter Expression and Rule Filter Actions](#Exercise2)
1. [Sending and Receiving Messages](#Exercise3)

Estimated time to complete this lab: **60 minutes**.

> **Note:** When you first start Visual Studio, you must select one of the predefined settings collections. Every predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options. The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection. If you choose a different settings collection for your development environment, there may be differences in these procedures that you need to take into account.

<a name="Exercise1"></a>
### Exercise 1: Creating a Topic and Adding Subscriptions ###

In this exercise, you will learn how to create a Windows Azure Service Bus topic and add subscriptions to it. Topics and subscriptions provide a one-to-many form of communication, in a “publish/subscribe” pattern. Useful for scaling to very large numbers of recipients, each published message is made available to each subscription registered with the topic.

<a name="Ex1Task1"></a>
#### Task 1 - Creating your Service Bus Namespace ####

To work with Service Bus topics and subscriptions, you first need to create a Windows Azure Service Bus namespace. Once created, it can be used for **all** of the labs that use Windows Azure Service Bus and for your own projects as well.

1. Navigate to [http://manage.windowsazure.com/](http://manage.windowsazure.com). You will be prompted for your **Microsoft Account** credentials if you are not already signed in.

1. Click **Service Bus** within the left pane.

	![Configuring Windows Azure Service Bus](Images/configuring-windows-azure-service-bus.png?raw=true)
 
	_Configuring Windows Azure Service Bus_

1. Create a Service Namespace. A service namespace provides an application boundary for each application exposed through the Service Bus and is used to construct Service Bus endpoints for the application. To create a service namespace, click **Create** on the bottom bar. 

 	![Creating a new namespace](Images/creating-a-new-namespace.png?raw=true)
 
	_Creating a new namespace_

1. In the **Create A Namespace** dialog box, enter a name for your service **Namespace** and select a **Region** for your service to run in. Service names must be globally unique as they are hosted in the cloud and accessible by whomever you decide to grant access.

 	![Create A Namespace Dialog Box](Images/create-a-namespace-dialog-box.png?raw=true)
 
	_Create A Namespace dialog box_

	> **Note:** It can take a few minutes while your service is provisioned.

1. Once the namespace is active, select the service's row and click **Connection Information** within the bottom menu.

	![Connection information](Images/connection-information.png?raw=true)

	_View connection information_

1. In the **Access connection information** dialog box, record the value shown for **Default Issuer** and **Default Key**, and click **OK**. You will need these values later when connecting to Service Bus from Visual Studio and when configuring your web role settings.

 	![Service Bus default issuer and key](Images/service-bus-default-issuer-and-key.png?raw=true)
 
	_Service Bus default keys_

You have now created a new Windows Azure namespace for this hands-on lab. To sign in at any time, simply navigate to the Windows Azure Management Portal, click **Sign In** and provide your **Microsoft Account** credentials.

> **Note:** In this lab you will learn how to create and make use of Service Bus topics and subscriptions from Visual Studio and from an ASP.NET MVC application. You can also create topics and subscriptions from the Windows Azure Management Portal, for more information see [How to Manage Service Bus Messaging Entities](http://www.windowsazure.com/en-us/documentation/articles/service-bus-manage-message-entities/).

<a name="Ex2Task2"></a>
#### Task 2 - Creating a Topic and Adding Subscriptions in Visual Studio ####
The Windows Azure Tools for Microsoft Visual Studio includes Server Explorer support for managing Service Bus messaging entities, including topics and subscriptions. In this task, you will use Server Explorer to connect to the service bus namespace you created previously, create a topic and add a subscription to it.

1. Open **Visual Studio 2013 Express for Web** (or greater) as Administrator.

1. From the menu bar, select **View** and then click **Server Explorer**.

1. In **Server Explorer**, expand the  **Windows Azure** node, right-click **Service Bus** and select **Add New Connection...**.

	![Adding new Service Bus connection](Images/adding-new-service-bus-connection.png?raw=true)

	_Adding new Service Bus connection_

1. In the **Add Connection** dialog box, make sure the **Windows Azure Service Bus** option is selected. Enter the **Namespace name**, the **Issuer Name** and the **Issuer Key** using the values obtained in the previous task. Finally, click **OK**.

	> **Note:** Alternatively, you can check the **Use connection string** checkbox and provide the service bus connection string.

	![Add Connection dialog box](Images/add-connection-dialog-box.png?raw=true)

	_Add Connection dialog box_

1. After connecting to your Service Bus namespace, your namespace should appear under **Service Bus**. Expand the Service Bus namespace node, right-click **Topics** and select **Create New Topic...**. 

	![Creating new Topic](Images/creating-a-new-topic.png?raw=true)

	_Creating new topic_

1. In the New Topic dialog, enter a name for the service bus topic in the **Name** textbox. Leave the default options and click **Save**.

	![New Topic dialog box](Images/new-topic-dialog-box.png?raw=true)

	_New Topic dialog box_

1. The new topic should be added under **Topics**. Expand the topic node, right-click  **Subscriptions** and select **Create New Subscription...**.

	![Creating new subscription](Images/creating-new-subscription.png?raw=true)

	_Creating new subscription_

1. In the **New Subscription** dialog box, enter a Name for the subscription in the **Name** textbox. Leave the default options and click **Save**.

	![New Subscription dialog box](Images/new-subscription-dialog-box.png?raw=true)

1. The new subscription should be added to your topic.

	> **Note:** You can also use the Windows Azure Tools for Microsoft Visual Studio to send and receive test messages, as well as to define subscription rules. In the next exercises, you will learn how to perform those operations from code by using the **WindowsAzure.ServiceBus** NuGet package.

	![New subscription created](Images/new-subscription-created.png?raw=true)

	_New subscription created_

<a name="Ex2Task3"></a>
#### Task 3 - Creating a Topic and Adding Subscriptions Programmatically ####

In this task, you will learn how to use the **Mircosoft.ServiceBus.NamespaceManager** class to create a new topic and add several subscriptions to it. For this, first you will add the necessary configurations to connect to your Service Bus namespace.

1. Open **Visual Studio 2013 Express for Web** (or greater) as Administrator.

1. Open the **Begin.sln** solution file from **Source\Ex1-CreatingATopicAndAddingSubscriptions\Begin\**.

1. Build the solution in order to download and install the NuGet package dependencies. To do this, in **Solution Explorer** right-click the solution node and select **Build Solution** or press **Ctrl + Shift + B**.

	>**Note:** NuGet is a Visual Studio extension that makes it easy to add, remove, and update libraries and tools in Visual Studio projects that use the .NET Framework.
	>
	> When you install the package, NuGet copies files to your solution and automatically makes whatever changes are needed, such as adding references and changing your app.config or web.config file. If you decide to remove the library, NuGet removes files and reverses whatever changes it made in your project so that no clutter is left.
	>
	>For more information about NuGet, visit [http://nuget.org/](http://nuget.org/).

1. Update the service configuration to define the configuration settings required to access your Service Bus namespace. To do this, in **Solution Explorer** expand the **Roles** folder in the **UsingTopics** project, right-click **UsingTopics** and then select **Properties**.

 	![Launching the Service Configuration editor](Images/launching-the-service-configuration-editor.png?raw=true)
 
    _Launching the Service Configuration editor_

1. In the **Settings** tab, set _namespaceAddress_ value to the name of your Service Bus namespace, and set the _issuerName_ and _issuerKey_ values to the ones you previously copied from the [Windows Azure Management Portal](http://go.microsoft.com/fwlink/?LinkID=129428).

	![Updating settings to the UsingTopics web role](Images/updating-settings-to-the-usingtopics-web-role.png?raw=true)

    _Updating settings to the **UsingTopics** web role_

1. Press **CTRL + S** to save the changes to the Web Role configuration.

1. Next, you will add the required assemblies to the **ASP.NET MVC 5** Web project to connect to the Windows Azure service bus from your application. In **Solution Explorer**, right-click the **UsingTopics** project node and select **Add Reference.**

1. In the **Reference Manager** dialog box, check the **System.Runtime.Serialization** assembly. Then, select the **Extensions** assemblies from the left pane, check **Microsoft.ServiceBus** and ensure **Microsoft.WindowsAzure.ServiceRuntime** is checked as well. Click **OK** to add the references.

1. Open the **HomeController.cs** file under the **Controllers** folder in the **UsingTopics.Web** project.

1. Add the following namespace directives to declare the Service Bus and the Windows Azure supporting assemblies.

	(Code Snippet - _Service Bus Topics - Ex01 - Adding Namespace Directives_ - CS)
	<!-- mark:1-2 -->
	````C#
	using Microsoft.ServiceBus;
	using Microsoft.WindowsAzure.ServiceRuntime;
	````
1. Add the following property to the **HomeController** class to enable the communication with the Service Bus Namespace service.

	(Code Snippet - _Service Bus Topcis - Ex01 - NamespaceManager Property_ - CS)
	<!-- mark:1-1 -->
	````C#
	private NamespaceManager namespaceManager;
	````

1. In order to create a topic, you have to connect to the **Service Bus Namespace** address and bind this namespace to a **NamespaceManager**. This class is in charge of creating the entities responsible for sending and receiving messages through topics. Add a constructor for the **HomeController** by including the following code:

	(Code Snippet - _Service Bus Topics - Ex01 - HomeController Constructor_ - CS)
	<!-- mark:1-10 -->
	````C#
	public HomeController()
	{
		 var baseAddress = RoleEnvironment.GetConfigurationSettingValue("namespaceAddress");
		 var issuerName = RoleEnvironment.GetConfigurationSettingValue("issuerName");
		 var issuerKey = RoleEnvironment.GetConfigurationSettingValue("issuerKey");

		 Uri namespaceAddress = ServiceBusEnvironment.CreateServiceUri("sb", baseAddress, string.Empty);

		 this.namespaceManager = new NamespaceManager(namespaceAddress, TokenProvider.CreateSharedSecretTokenProvider(issuerName, issuerKey));
	}
	````

1. You will use the **namespaceManager** object to create a new **topic** with two **subscriptions**, named _AllMessages_, and _UrgentMessages_. To do this, add the following method at the end of the **HomeController** class.

	(Code Snippet - _Service Bus Topics - Ex01 - Create Topic and subscriptions_ - CS)
	<!-- mark:1-19 -->
	````C#
	[HttpPost]
	public JsonResult CreateTopic(string topicName)
	{
		 var topic = this.namespaceManager.CreateTopic(topicName);
		 var allMessagesSubscription = this.namespaceManager.CreateSubscription(topic.Path, "AllMessages");
		 var urgentMessagesSubscription = this.namespaceManager.CreateSubscription(topic.Path, "UrgentMessages");

		 return this.Json(topicName, JsonRequestBehavior.AllowGet);
	}
	````

1. Add the following code at the end of the **HomeController** class to retrieve the topics and subscriptions data to the view.

	(Code Snippet - _Service Bus Topics - Ex02 - GetTopics and Subscriptions_ - CS)
	<!-- mark:1-13 -->
	````C#
	[OutputCache(NoStore = true, Duration = 0, VaryByParam = "*")]
	public JsonResult Topics()
	{
		 var topics = this.namespaceManager.GetTopics().Select(c => c.Path);
		 return this.Json(topics, JsonRequestBehavior.AllowGet);
	}

	[OutputCache(NoStore = true, Duration = 0, VaryByParam = "*")]
	public JsonResult Subscriptions(string topicName)
	{
		 var subscriptions = this.namespaceManager.GetSubscriptions(topicName).Select(c => new { Name = c.Name, MessageCount = c.MessageCount });
		 return this.Json(subscriptions, JsonRequestBehavior.AllowGet);
	}
	````

1. Press **CTRL + S** to save the changes to the Controller.

<a name="Ex1Verification"></a>
#### Verification ####
You will now launch the updated application in the Windows Azure compute emulator to verify that you can create a topic with subscriptions.

1. In **Visual Studio**, configure the cloud project **UsingTopics.Azure** as the StartUp Project. To do this, in **Solution Explorer** right-click the **UsingTopics.Azure** project node and then select **Set as StartUp Project**.

	![Configuring StartUp project](Images/configuring-startup-project.png?raw=true)

	_Configuring StartUp project_

1. Press **F5** to launch the application. The browser will show the default page of the application (note that the topic you created in the previous task is displayed under **Topic Explorer**).

	![Service Bus Topics application home page](Images/service-bus-topics-application-home-page.png?raw=true)

	_Service Bus Topics application home page_

1. In the **Create a Topic** section, enter _SimpleTopic_ for the topic name, and click **Create**.

	![Creating a topic](Images/creating-a-topic.png?raw=true)

	_Creating a topic_

1. The application calls Service Bus to create the topic. The topic is added to the topics list and a successful message is displayed.

	![Topic created](Images/topic-created.png?raw=true)

	_Topic created_

1. Select the new topic from the topic list. The application will retrieve the subscriptions associated to the topic from Service Bus.

	![Retrieving topic subscriptions](Images/retrieving-topic-subscriptions.png?raw=true)

	_Retrieving topic subscriptions_

1. Go back to **Visual Studio** and stop debugging.

<a name="Exercise2"></a>
### Exercise 2: Sending and Receiving Messages ###

// TODO: Add intro paragraph.

<a name="Ex2Task1"></a>
#### Task 1 - Sending Messages ####

In this task, you will send messages through a topic and verify that each message arrives to all subscriptions. You can send any serializable object as a **Message** through topics. You will send a **CustomMessage** object which has its own properties and is agnostic on how the Service Bus topic works or interacts with your application.

1. If not already opened, open the **HomeController.cs** file under the **Controllers** folder in the **UsingTopics** project.

1. Create a new class under the **Models** folder of the **UsingTopics** project. To do this, right-click the folder, select **Add** and then **Class**. In the **Add New Item** dialog box, set the name of the class to _CustomMessage_.

1. Replace the entire code of the class with the following code.

	(Code Snippet - _Service Bus Topics - Ex02 - CustomMessage Class_ - CS)
	<!-- mark:1-23 -->
	````C#
	namespace UsingTopics.Models
	{
		 using System;

		 [Serializable]
		 public class CustomMessage
		 {
			  private DateTime date;
			  private string body;

			  public DateTime Date
			  {
					get { return this.date; }
					set { this.date = value; }
			  }

			  public string Body
			  {
					get { return this.body; }
					set { this.body = value; }
			  }
		 }
	}
	````

1. Press **CTRL + S** to save the changes.

1. Open the **HomeController.cs** file under the **Controllers** folder in the **UsingTopics** project.

1. Add the following namespace directive to access the Service Bus messaging classes.

	(Code Snippet - _Service Bus Topics - Ex02 - Adding Namespace Directives_ - CS)
	<!-- mark:1-1 -->
	````C#
	using Microsoft.ServiceBus.Messaging;
	````

1. Add the following property to the **HomeController** class to enable access to the Service Bus messaging capabilities.

	(Code Snippet - _Service Bus Topics - Ex02 - MessagingFactory Property_ - CS)
	<!-- mark:1-1 -->
	````C#
	private MessagingFactory messagingFactory;
````

1. In order to send a message you have to bind the **Service Bus Namespace** address to a **MessagingFactory**. This is the anchor class used for run-time operations to send and receive messages to and from topics or subscriptions. Add the following code at the end of the **HomeController** constructor.

	(Code Snippet - _Service Bus Topics - Ex02 - Creating MessagingFactory_ - CS)
	<!-- mark:5-5 -->
	````C#
	public HomeController()
	{
		...

		this.messagingFactory = MessagingFactory.Create(namespaceAddress, TokenProvider.CreateSharedSecretTokenProvider(issuerName, issuerKey));
	}
	````

1. Next, you will create the method in the **HomeController** class that allows you to send your custom object to a **topic**. Add the following method to the class.

	(Code Snippet - _Service Bus Messaging - Ex02 - SendMessage_ - CS)
	<!-- mark:1-31 -->
	````C#
	[HttpPost]
	public JsonResult SendMessage(string topicName, string message, bool messageIsUrgent, bool messageIsImportant)
	{
	    TopicClient topicClient = this.messagingFactory.CreateTopicClient(topicName);
	    var customMessage = new CustomMessage() { Body = message, Date = DateTime.Now };
	    bool success = false;
	    BrokeredMessage bm = null;
	
	    try
	    {
	        bm = new BrokeredMessage(customMessage);
	        bm.Properties["Urgent"] = messageIsUrgent ? "1" : "0";
	        bm.Properties["Important"] = messageIsImportant ? "1" : "0";
	        bm.Properties["Priority"] = "Low";
	        topicClient.Send(bm);
	        success = true;
	    }
	    catch (Exception)
	    {
	        // TODO: do something
	    }
	    finally
	    {
	        if (bm != null)
	        {
	          bm.Dispose();
	        }
	    }
	
	    return this.Json(success, JsonRequestBehavior.AllowGet);
	}
	````

1. Press **CTRL + S** to save the changes to the Controller.

<a name="Ex2Task2"></a>
#### Task 2 - Receiving Messages ####

In this task, you will learn how to receive messages from a subscription. You will use a very similar logic used in Exercise 1, but in this case you will instantiate a **MessageReceiver** object from a **SubscriptionClient**.

1. If not already opened, open the **HomeController.cs** file under the **Controllers** folder in the **UsingTopics.Web** project.

1. Add the following code at the end of the **HomeController** class.

	(Code Snippet - _Service Bus Messaging - Ex02 - RetrieveMessages_ - CS)
	<!-- mark:1-34 -->
	````C#
	[HttpGet, OutputCache(NoStore = true, Duration = 0, VaryByParam = "*")]
	public JsonResult RetrieveMessage(string topicName, string subscriptionName)
	{
	    SubscriptionClient subscriptionClient = this.messagingFactory.CreateSubscriptionClient(topicName, subscriptionName, ReceiveMode.PeekLock);
	    BrokeredMessage receivedMessage = subscriptionClient.Receive(new TimeSpan(0,0,30));
	
	    if (receivedMessage == null)
	    {
	        return this.Json(null, JsonRequestBehavior.AllowGet);
	    }
	
	    var receivedCustomMessage = receivedMessage.GetBody<CustomMessage>();

		receivedMessage.Properties["Priority"] = receivedMessage.Properties["Important"].ToString() == "1" ? "High" : "Low";

	    var brokeredMsgProperties = new Dictionary<string, object>();
	    brokeredMsgProperties.Add("Size", receivedMessage.Size);
	    brokeredMsgProperties.Add("MessageId", receivedMessage.MessageId.Substring(0, 15) + "...");
	    brokeredMsgProperties.Add("TimeToLive", receivedMessage.TimeToLive.TotalSeconds);
	    brokeredMsgProperties.Add("EnqueuedTimeUtc", receivedMessage.EnqueuedTimeUtc.ToString("yyyy-MM-dd HH:mm:ss"));
	    brokeredMsgProperties.Add("ExpiresAtUtc", receivedMessage.ExpiresAtUtc.ToString("yyyy-MM-dd HH:mm:ss"));
	
	    var messageInfo = new
	    {
	        Label = receivedMessage.Label,
	        Date = receivedCustomMessage.Date,
	        Message = receivedCustomMessage.Body,
	        Properties = receivedMessage.Properties.ToArray(),
	        BrokeredMsgProperties = brokeredMsgProperties.ToArray()
	    };
	
	    receivedMessage.Complete();
	    return this.Json(messageInfo, JsonRequestBehavior.AllowGet);
	}
	````

	> **Note:** In this code you are also adding additional information of the message that you will show in the UI.

1. Add the following code at the end of the **HomeController** class to retrieve the topics and subscriptions data to the View.

	(Code Snippet - _Service Bus Messaging - Ex02 - GetTopic and Subscriptions_ - CS)
	<!-- mark:1-49 -->
	````C#
	[OutputCache(NoStore = true, Duration = 0, VaryByParam = "*")]
	public JsonResult Subscriptions(string topicName)
	{
	    var subscriptions = this.namespaceManager.GetSubscriptions(topicName).Select(c => c.Name);
	    return this.Json(subscriptions, JsonRequestBehavior.AllowGet);
	}
	
	[OutputCache(NoStore = true, Duration = 0, VaryByParam = "*")]
	public JsonResult TopicsWithSubscriptions()
	{
	    var topics = this.namespaceManager.GetTopics().Select(c => c.Path).ToList();
	    var topicsToReturn = new Dictionary<string, object>();
	    topics.ForEach(c =>
	    {
	        var subscriptions = this.namespaceManager.GetSubscriptions(c).Select(d => new { Name = d.Name, MessageCount = d.MessageCount });
	        topicsToReturn.Add(c, subscriptions);
	    });
	
	    return this.Json(topicsToReturn.ToArray(), JsonRequestBehavior.AllowGet);
	}
	
	[OutputCache(NoStore = true, Duration = 0, VaryByParam = "*")]
	public JsonResult Filters(string topicName, string subscriptionName)
	{
	    var rules = this.namespaceManager.GetRules(topicName, subscriptionName);
	    var sqlFilters = new List<Tuple<string, string>>();
	
	    foreach (var rule in rules)
	    {
	        var expression = rule.Filter as SqlFilter;
	        var action = rule. Action as SqlRuleAction;
	
	        if (expression != null)
	        {
	            sqlFilters.Add(
	                new Tuple<string, string>(
	                    expression.SqlExpression,
	                    action != null ? action.SqlExpression : string.Empty));
	        }
	    }
	
	    return this.Json(sqlFilters.Select(t => new { Filter = t.Item1, Action = t.Item2 }), JsonRequestBehavior.AllowGet);
	}
	
	public long GetMessageCount(string topicName, string subscriptionName)
	{
	    var subscriptionDescription = this.namespaceManager.GetSubscription(topicName, subscriptionName);
	    return subscriptionDescription.MessageCount;
	}
	````

	> **Note:** These methods are used by the View to retrieve the information on Topics and Subscriptions via jQuery and AJAX.

1. Press **CTRL + S** to save the changes to the Controller.

<a name="Exercise3"></a>
### Exercise 3: Using a Subscription Rule Filter Expression and Rule Filter Actions ###

In this exercise, you will apply filters on subscriptions to retrieve only the messages relevant to that subscription. When you send a message to a topic, all the subscriptions verify if the message has a match with its own subscription rules. If there is a match, the subscription will contain a virtual copy of the message. This is useful to avoid sending multiple messages to different subscriptions. Sending a single message to a topic will distribute along different subscriptions by checking **Rule Expressions**. Additionally, you will learn how to apply **Filter Actions** to subscriptions to modify the **BrokeredMessage** properties of the messages that match a custom rule.


<a name="Ex3Task1"></a>
#### Task 1 - Using a Subscription Rule Filter Expression ####

**Rule Filters** are used in Subscriptions to retrieve messages that match certain rules. That way you can send one message to a Topic, but it _virtually_ replicates through multiple Subscriptions.

1. If not already opened, open the **HomeController.cs** file under the **Controllers** folder in the **UsingTopics.Web** project.

1. In the previous task, you created a Topic with two Subscriptions. Now, you will replace a line of that code to include a **SqlFilter** to the _UrgentMessages_ Subscription. With this filter, the _UrgentMessages_ subscription will get only the messages that match the rule **Urgent = '1'**. Replace the code you added in the previous task with the following highlighted code.

	(Code Snippet - _Service Bus Messaging - Ex02 - Create Topic and Subscriptions with Rule Filters_ - CS)
	<!-- mark:9 -->
	````C#
	[HttpPost]
	public JsonResult CreateTopic(string topicName)
	{
	    bool success;
	    try
	    {
	        var topic = this.namespaceManager.CreateTopic(topicName);
	        var allMessagesSubscription = this.namespaceManager.CreateSubscription(topic.Path, "AllMessages");
	        var urgentMessagesSubscription = this.namespaceManager.CreateSubscription(topic.Path, "UrgentMessages", new SqlFilter("Urgent = '1'"));
	   ...
	}
	````

    > **Note:** Take into account that you can use SQL92 as Filter Expressions.

1. Press **CTRL + S** to save the changes to the Controller.

<a name="Ex3Task2"></a>
#### Task 2 - Using a Subscription Rule Filter Action ####

Additionally to Rule Filter Expressions, you can use **Rule Filter Actions.** With this, you can modify the properties of a **BrokeredMessage** that matches the specified rule. You will create a new **Subscription** named _HighPriorityMessages_ containing a custom **Rule Filter Action**. All messages that match the rule _Urgent = '1'_ will be sent to that **Subscription** with the property **Priority** set to _'High'_.

> **Note:** Both Filter Expressions and Filter Actions use the properties declared in the **BrokeredMessage** dictionary named **Properties**. These rules won't apply on custom objects inside the body of the **BrokeredMessage.**

1. If not already opened, open the **HomeController.cs** file under the **Controllers** folder in the **UsingTopics.Web** project.

1. Create a new **Subscription** object with a **RuleDescription**. Within this object, you can set a **Filter** and an **Action**. This way, if the **Filter** matches, the specific **Action** is applied to the **BrokeredMessage**. In the **CreateTopic** Action Method, add the highlighted code.

	(Code Snippet - _Service Bus Messaging - Ex02 - Create Subscription with Action Filter_ - CS)
	<!-- mark:11-16 -->
	````C#
	[HttpPost]
	public JsonResult CreateTopic(string topicName)
	{
	    bool success;
	    try
	    {
	        var topic = this.namespaceManager.CreateTopic(topicName);
	        var allMessagesSubscription = this.namespaceManager.CreateSubscription(topic.Path, "AllMessages");
	        var urgentMessagesSubscription = this.namespaceManager.CreateSubscription(topic.Path, "UrgentMessages", new SqlFilter("Urgent = '1'"));
	
	        var ruleDescription = new RuleDescription()
	        {
	            Filter = new SqlFilter("Important= '1' OR Priority = 'High'"),
	            Action = new SqlRuleAction("set Priority= 'High'")
	        };
	        var highPriorityMessagesSubscription = this.namespaceManager.CreateSubscription(topic.Path, "HighPriorityMessages", ruleDescription);
	        success = true;
	    }
	    catch (Exception)
	    {
	        success = false;
	    }
	
	    return this.Json(success, JsonRequestBehavior.AllowGet);
	}
	````

1. Press **CTRL + S** to save the changes to the Controller.

<a name="Ex2Verification"></a>
#### Verification ####

You will now launch the updated application in the Windows Azure compute emulator to verify that you can create a Topic with subscriptions, send and receive messages. You will verify that each message will go to the subscription that matches the correct filter.

1. In **Visual Studio**, configure the cloud project **UsingTopics** as the StartUp Project. To do this, in the **Solution Explorer**, right-click on **UsingTopics** and then select **Set as StartUp Project**.

 	![Configuring StartUp Project](./Images/setting-startup-project2.png?raw=true "Configuring StartUp Project")
 
	_Configuring StartUp Project_

1. Press **F5** to launch the application. The browser will show the default page of the application.

 	![UsingTopics Application Home Page](./Images/UsingTopics-Application-Home-Page.png?raw=true "UsingTopics Application Home Page")
 
	_UsingTopics Application Home Page_

1. In the panel named **Topics**, enter a topic name, for example _MyTopic_, and click **Create**.

 	![Creating a Topic](./Images/Creating-a-Topic.png?raw=true "Creating a Topic")
 
	_Creating a Topic_

 	![The application displays a message when a Topic is created](./Images/The-application-displays-a-message-when-a-Topic-is-created.png?raw=true "The application displays a message when a Topic is created")
 
	_The application displays a message when a Topic is created_

1. In the **Send Message** panel, select the previously created **Topic** from the dropdown list, enter "This is an urgent message" in the TextBox, check **Is Urgent** and click **Send.**
 
 	![Sending a message to the topic](./Images/Sending-a-message-to-the-topic.png?raw=true "Sending a message to the topic")
 
	_Sending a message to the topic_

1. Check that the message is received only by the **UrgentMessages** and the **AllMessages** subscriptions. To do this, select each subscription in the dropdown list located in the **Receive Message** panel and click **Retrieve First message in Subscription**.

 	![Retrieving a message to the AllMessages subscription](./Images/Retrieving-a-message-to-the-AllMessages-subscription.png?raw=true "Retrieving a message to the AllMessages subscription")
 
	_Retrieving a message to the AllMessages subscription_

 	![Retrieving a message to the HighPriorityMessages subscription](./Images/Retrieving-a-message-to-the-HighPriorityMessages-subscription.png?raw=true "Retrieving a message to the HighPriorityMessages subscription")
 
	_Retrieving a message to the HighPriorityMessages subscription_

 	![Retrieving a message to the UrgentMessages subscription](./Images/Retrieving-a-message-to-the-UrgentMessages-subscription.png?raw=true "Retrieving a message to the UrgentMessages subscription")
 
	_Retrieving a message to the UrgentMessages subscription_

1. Send another message to the Topic, but this time, uncheck the **Is Urgent** checkbox and check **Mark as important**. Retrieve the message from the **HighPriorityMessages** subscription and verify that the Priority is now set to **High**.

 	![Sending an important message to the Topic](./Images/Sending-an-important-message-to-the-Topic.png?raw=true "Sending an important message to the Topic")
 
 	_Sending an important message to the Topic_

---

<a name="Summary"></a>
## Summary ##

 By completing this hands-on lab, you have reviewed the basic elements of Service Bus Queues, Topics and Subscriptions. You have seen how to send and retrieve messages through a Queue and how to create Topics and Subscriptions to it. Finally, you learned how to apply Expression Filters and Rule Actions to Subscriptions to distribute your messages that matched those rules.
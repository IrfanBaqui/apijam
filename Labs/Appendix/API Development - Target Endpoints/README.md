# API Development : Externalize Target URLs using Target Servers

*Duration : 20 mins*

*Persona : API Team*

# Use case

Backend target URLs often change as an API is promoted from development through testing and finally into production. The ability to externalize these endpoints ensures you can take your API configuration and promote it through your SDLC without making manual changes as part of your deployment/promotion process.

# How can Apigee Edge help?

Apigee Edge includes the ability to externalize your backend target URL through a concept known as *Target Servers*. Target Servers are configured for each environment and allow you to replace a static target URL in your API Proxy definition with a named target server that is automatically replaced at runtime.

# Pre-requisites

None

# Instructions

* Go to [https://apigee.com/edge](https://apigee.com/edge) and log in. This is the Edge management UI.

* On the left hand menu, navigate to **Admin --> Environments --> Target Servers**:

![Select Target Servers](./media/image_1.png)

* Ensure that the **test** environment is selected, then click on **+ Target server**

![Add Target Server](./media/image_2.png)

* Enter the following values for your test environment's target server, then save your configuration:

  * Enabled: checked

  * Name: {your-initials}-employees-service

  * Host: cosafinity-test.apigee.net

  * Port: 80

![Create test target server](./media/image_3.png)

You should see a message indicating your target server was created successfully and your new target server should be displayed on the screen.

* Using the environment drop down, change to the *prod* environment:

![switch to prod environment](./media/image_4.png)

* Once again, add another target server. Use the following configurations:

  * Enabled: checked

  * Name: {your-initials}-employees-service

  * Host: cosafinity-prod.apigee.net

  * Port: 80

![Click on edit](./media/image_5.png)

At this point we now have created our target server for the *Employees Service* in both the test and prod environments. Our next task will be configuring a proxy that uses the named target servers.

* Navigate to the API Proxies list view and select the **Employee** proxy you created previously

![Proxies list](./media/open-api-proxies-list.png)

* In the develop tab, select the *default* Target Connection from the left hand pane. Scroll down to the bottom of the XML configuration until you see a tag labeled `HTTPTargetConnection`:

![Find Target Connection](./media/find-target-connection.png)

* Modify the `HTTPTargetConnection` to use a Load Balancer configuration as shown below. You can copy and paste the configuration provided below the image, making sure to replace {your-initials} with your actual initials. Click Save to save the proxy changes.

![Configure Target](./media/configure-proxy-target-endpoint.png)

You can copy/paste the following configuration for your `HTTPTargetConnection`:

```
  <HTTPTargetConnection>
    <LoadBalancer>
      <Server name="{your-initials}-employees-service" />
    </LoadBalancer>
    <Path>/v1/employees</Path>
  </HTTPTargetConnection>
```

* Click on the Deployment drop down and select the prod environment to deploy your proxy to prod. This will bring up a confirmation dialog, click on the *Deploy* button to deploy.

![Deploy to Prod](./media/deploy-to-prod.png)

* Click on the Overview tab to verify that your API Proxy is deployed to both test and prod:

![Click on Overview](./media/click-on-overview.png)

* On the Overview tab, click on the test and prod URLs to call the API in each environment.

![Click on URLs](./media/call-prod-and-test-apis.png)

* Verify you get a JSON response from both the test and prod APIs. Then, open up the **trace** console and confirm that traffic is routed to the correct backend endpoint. If you do, congrats, you've successfully configured an API which relies on named target servers instead of hard coded URLs!

# Lab Video

[Configuring Target Servers](https://www.youtube.com/watch?v=crnuHAgj9Vo&index=18&list=PLIXjuPlujxxxe3iTmLtgfIBgpMo7iD7fk)

# Earn Extra Points

In this lab, we configured a single target server, but Apigee Edge can actually [load balance between multiple target servers](http://docs.apigee.com/api-services/content/load-balancing-across-backend-servers). Try some of these more advanced options as well:
* Add a second target server and configure the load balancer to send traffic equally between th e two target servers.
* Configure a weighted load balancer configuration to send 2 times the traffic to one target server versus the other
* Disable one target server and experiment with failover behavior
* Try creating a target server from the [management API](http://docs.apigee.com/management/apis/post/organizations/%7Borg_name%7D/environments/%7Benv_name%7D/targetservers)

# Quiz

1. How might target servers be useful as part of your CI/CD process?

2. What additional steps would you need to think about if you didn't have the ability to create named target servers in Apigee Edge?

# Summary

In this lab, we saw how target URLs can be externalized using Target Servers to implement routing to the correct backend server based on the environment an API is deployed to. This helps to make it easy to configure Apigee Edge to work in a way that conforms to your SDLC.

# References

* [Target Server and load balancing documentation](http://docs.apigee.com/api-services/content/load-balancing-across-backend-servers)
* [Management API documentation for creaating Target Servers](http://docs.apigee.com/management/apis/post/organizations/%7Borg_name%7D/environments/%7Benv_name%7D/targetservers)

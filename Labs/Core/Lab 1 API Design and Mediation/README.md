# API Design : Create a Reverse Proxy with OpenAPI Specification

*Duration : 20 mins*

*Persona : API Team*

# Use case

You have a requirement to create a reverse proxy for taking requests from the Internet and forward them to an existing service. You have decided to follow a design first approach & built a reusable component, a specification which can be used to build API Proxies, generate API documentation, generate API test cases using OpenAPI Specification format. You would like to generate an Apigee API Proxy by using the OpenAPI Specification (Swagger) instead of building the API Proxy from scratch.

# How can Apigee Edge help?

Apigee Edge enables you to quickly expose backend services as APIs. You do this by creating an API proxy that provides a facade for the backend service that you want to expose. Apigee Edge out of the box supports the OpenAPI specification, allowing you to auto-generate API Proxies. Apigee Edge also has an OpenAPI specification editor & store which you can use to maintain your OpenAPI specifications. 

The API proxy decouples your backend service implementation from the API that developers consume. This shields developers from future changes to your backend services. As you update backend services, developers, insulated from those changes, can continue to call the API uninterrupted.

In this lab, we will see how to create a reverse proxy, that routes inbound requests to existing HTTP backend services using a readily available OpenAPI specification.

# Part A - Design an API Proxy
## Import an Open API Specification

1. Go to [https://apigee.com/edge](https://apigee.com/edge) and log in. This is the Edge management UI. 

2. Select **Develop → Specs** in the side navigation menu

![image alt text](./media/image_0.png)

3. Click **+Spec.** Click on **New** to add a new spec from the UI.

4. Copy the spec details from [here](./resources/litmos-api-spec.yaml) and paste it on the left pane. 

5. Edit the **basePath** from `/v1` to `/v1/{your_initials}`

6. Hit **Save** on the top right.

   * File Name: **{your-initials}**_litmos_api_spec

7. Click on **{your-initials}**_litmos_api_spec from the list to access Open API spec editor & interactive documentation that lists API details & API Resources.

## Create an API Proxy

1. It’s time to create Apigee API Proxy from Open API Specification. Click on **Develop → API Proxies** from side navigation menu.

![image alt text](./media/image_5.jpg)

2. Click **+Proxy** The Build a Proxy wizard is invoked. 

![image alt text](./media/image_6.jpg)

3. Select **Reverse proxy**, Click on **Use OpenAPI** below reverse proxy option.

![image alt text](./media/image_7.png)

4. You should see a popup with list of Specs. Select **{your-initials}**_litmos_api_spec and click **Select.** 

5. You can see the selected OpenAPI Spec URL below the Reverse Proxy option, Click **Next** to continue.

![image alt text](./media/image_9.png)

6. Enter details in the proxy wizard. Replace **{your-initials}** with the initials of your name. 

    * Proxy Name: `{your_initials}_user_proxy`

    * Proxy Base Path: `/v1/{your_initials}/users`

    * Existing API: `https://apibeta.litmos.com/v1.svc/users`


7. Verify the values and click **Next**.

8. You can select, de-select list of API Proxy Resources that are pre-filled from OpenAPI Spec. Select only the `users` APIs & Click on **Next**.

9. Select **Pass through (none)** for the authorization in order to choose not to apply any security policy for the proxy. Click Next. 

![image alt text](./media/image_12.jpg)

10. Go with the **default Virtual Host** configuration.

![image alt text](./media/image_13.jpg)

11. Ensure that only the **test** environment is selected to deploy to and click **Build and Deploy**

12. Once the API proxy is built and deployed **click** the link to view your proxy in the proxy editor. 

13. *Congratulations!* ...You have now built a reverse proxy for an existing backend service. You should see the proxy **Overview** screen.

![image alt text](./media/image_16.png)

# Part B - Perform API Traffic Mediation
## Add query params to the request
1. Click on the **Postflow**. We will add a new `Assign Message` policy here to add three query params necessary for our backend.

2. Click on the **+** icon to the right in the request flow. From the list, click on the `Assign Message` policy. Name it **Add-Identification**.

3. Replace the policy configuration with the following:
```
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<AssignMessage async="false" continueOnError="false" enabled="true" name="Add-Identification">
    <DisplayName>Add-Identification</DisplayName>
    <Properties/>
    <Add>
        <QueryParams>
            <QueryParam name="apikey">4F1D0552-A887-427A-904D-7F08D94CDF05</QueryParam>
            <QueryParam name="source">googletest</QueryParam>
            <QueryParam name="format">json</QueryParam>
        </QueryParams>
    </Add>
    <IgnoreUnresolvedVariables>true</IgnoreUnresolvedVariables>
    <AssignTo createNew="false" transport="http" type="request"/>
</AssignMessage>
```

4. Click **save** on the top left to save the proxy. You have just made a fully working API Proxy!

## Test the API Proxy
1. Click on the **`Trace`** button on the top right of the proxy editor.

2. It should automatically paste the url of your proxy in the text field. If not, use the url `https://apijams-amer-10-test.apigee.net/v1/{your_initials}/users`

3. Click on the **Trace** button to start tracing calls.

4. Hit **Send**.

5. Inspect the trace on the screen.

## Save the API Proxy

1. Let’s save the API Proxy locally as an API Bundle so that we can reuse it in other labs.

2. Save the API Proxy by downloading the proxy bundle, See screenshot below for instructions.

![image alt text](./media/image_20.png)

# Quiz

1. How do you import the proxy bundle you just downloaded? 
2. How does Apigee Edge handle API versioning? 
3. Are there administrative APIs to create, update or delete API proxies in Apigee?

# Summary

That completes this hands-on lesson. In this simple lab you learned how to create a proxy for an existing backend using OpenAPI Specification and Apigee Edge proxy wizard.

# References

* Useful Apigee documentation links on API Proxies - 

    * Build a simple API Proxy - [http://docs.apigee.com/api-services/content/build-simple-api-proxy](http://docs.apigee.com/api-services/content/build-simple-api-proxy) 

    * Best practices for API proxy design and development - [http://docs.apigee.com/api-services/content/best-practices-api-proxy-design-and-development](http://docs.apigee.com/api-services/content/best-practices-api-proxy-design-and-development) 

* Watch this 4minute video on "Anatomy of an API proxy" - [https://youtu.be/O5DJuCXXIRg](https://youtu.be/O5DJuCXXIRg) 

# Rate this lab

Now go to [Lab-2](../Lab%202%20Traffic%20Management%20-%20Throttle%20APIs)


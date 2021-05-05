## Variables

In case of using same base url again and again, better store it in a variable and reuse the value.
Follow the steps below-

1. Select the part of the address before /training and click **Set as variable > Set as a new variable**
2. Enter a Name with the specified domain as the Value.
3. Select Collection for the Scope, making sure the correct collection is selected.
4. Click **Set variable**
5. If the variable name was _"training_api"_, a demo endpoint address will now look like -
   `GET {{training_api}}/matches`

## Console Usage

1. Send a request to an endpoint
2. Open the **Console** from the bottom of the Postman window to see the address the request sent to.
3. Click on a request in the Console to see the full detail of what happened
   - Local & remote network addresses and ports
   - Request and response headers
   - Response body

## Authorization Key

While POSTing new data to the API, you will typically need to _authorize_ your requests with **Auth key**.

A **variable** can be used to minimize visibility of these sensitive credentials.

1. Open the **Authorization tab** for the request
2. Select _'Inherit auth from parent'_ from the Type drop-down list
3. In Collections on the left, select the parent student expert collection (click `…` and choose **Edit** if you're in the app).
4. Open the Authorization tab. Postman will add the API key details to the header for every request using the name `match_key` and the value specified by the referenced `email_key` variable.
5. Add a variable to the collection also via the Edit menu—choosing the Variables tab. Use the name `email_key` and enter your **email address** in both value fields.
   > Postman will now append your email address to each child request to identify you as the client.

## POST Body Data

1. In Body, select **raw** and choose **JSON** instead of **Text** in the drop-down list.
2. Enter the following sample JSON data including the enclosing curly braces:

   ```
   {
   	"match": "Cup Final",
   	"when": "{{$randomDateFuture}}",
   	"against": "Academical"
   }
   ```

3. Send the **POST** request to add new data.

## Dynamic Variables

There are lots of other dynamic variables you can use in your requests for values you want to _calculate at runtime_, or if you want to use demo data instead of real values.

> For example, the `{{randomDateFuture}}` is a dynamic variable.
> Postman will add a random future date when you send your request.

## PUT Data

1. Create a new request in the collection with a name
2. Specify method **PUT**
3. Specify endpoint `{{training_api}}/match`
4. Specify a match to update
5. In **Params** tab, add `match_id` in the **Key** column, and the specified id value that you want to filter it with
6. In **Body** tab, select **raw** option and choose **JSON** instead of **Text** in the drop-down. Enter the following JSON data including the enclosing curly braces:

   ```
   	{
   		"points": 3
   	}
   ```

7. Click **Send** to update with a new value of a specified match

## DELETE Data

1. Create a new request in the collection with a name
2. Specify method **DELETE**
3. Specify endpoint `{{training_api}}/match/:match_id`
   This request includes a **path parameter** with `/:match_id` at the end of the request address.
4. Open **Params** tab and as the value for the `match_id` parameter, enter the id of a match you added during this session when you sent the POST request.
5. Click **Send** to delete the record of specified match.

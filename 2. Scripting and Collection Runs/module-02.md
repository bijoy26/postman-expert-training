## Adding Test Scripts

The following test checks whether the response **status code** is `200`.

```
    pm.test('Status code is 200', function () {
     	pm.response.to.have.status(200);
	});
```

The `pm.test()` function will use the first **text string** parameter to output the test result in Postman.

### Usage

1. Create a new request
2. Go to **Tests** tab
3. Write down the _test script_
4. Save the request and click **Send**
5. Check the **'Test Results'** in the response area. Response area can be filtered by `Passed, Skipped` and `Failed` tests.

   > NOTE: Access **test code snippets** on the right of the test editing area.

## Variables

There are two kinds of variables in Postman-

1. **Environment Variables:** A set of variables that allow you to switch the context of your requests.
2. **Global variables:** A set of variables that are always available in a workspace.

## Passing Values

Some request might expect **query parameters**.

An **environment variable** can can be used as the parameter.

1. Create a new request
2. At the top-right of the request area, click the **eye** button
3. Choose **Add** or use **Environments** on the left for Postman _web version_.
4. Choose a _name_ and add a variable with the name `player_id`, leaving the value blank for now.
5. Select your new environment from the drop-down list at the top-right to make it the **active** environment. The new environment will be displayed and can be accessed.
6. Instead of setting the variable value manually, we can set it using a **script** added to the previous request so that it uses a _random player_ selected from the response.
7. Open the previous request and append the following to the test code:
   ```
   var player_list = pm.response.json().data.players;
   pm.environment.set('player_id', player_list[Math.floor(Math.random() * player_list.length)].id);`
   ```
   Here, from the response, the environment variable `player_id` is getting set with a random value from 1-3.
8. **Save** and **Send** the request.
9. Hover over the **Params** variable and verify the **Value** to see if the variable now has a value set from 1-3.
10. Now send the new request with the set `env var` as query parameter to obtain the the player data.

## Pre-request scripts

A request can also have **predefined** default value set as environment variable.
For that purpose-

1. Deselect the environment from drop-down list if any one is active. Any existing env var value will turn **RED** after this.
2. In the request _Pre-request script_ add the following code:

   `if(!pm.variables.get('player_id')) pm.variables.set('player_id', -1);`

   It sets a default value of `-1` only if there is not a value set yet.
   Send the previous request again and check the console to see that `id=-1` was sent.

3. Select the environment you created again from the drop-down list and hover over the var again. Now it should again have the value from the var you set from the first request.
4. Add a **description** to appear within the collection documentation, which is useful for publishing an API for public use. To perform that, expand the right sidebar, click on the top tab **documentation** and click to edit.

Add a **short description** of the request (you can use **markdown**) and click **Save**. 5. From the app, open the collection on the left using the arrow ► button and click `View documentation in web` to see your description in the collection docs (in the browser).

> Descriptions can be added to each request, to the collection as a whole, and to various other components.

## Running Collections

Postman **collection runner** can be used to run sequences of request in a collection.
Scripting can be added to use with the **collection runner**.

1. Add some _test code_ (status code checker) to each of the requests in a folder and save the requests to be reviewed in the collection runner.
2. Add another test to the current request to check that the **array includes all of the required stats fields**

   ```
   pm.test('Stats include all fields', function () {
   var jsonData = pm.response.json().data;
   pm.expect(jsonData).to.have.all.keys('won', 'lost', 'drew');
   });
   ```

3. Click **Save** and **Send**.
4. click **Runner**, select the _Student Training collection_ and specify the current folder, select the environment you used earlier, and run the collection. If you're on the _web version_ of Postman, select the specified folder on the left and click Run button on right.
5. Check the test results output including `passed` and `failed` status.
   In addition, use **Console** at the bottom left to drill down into requests and responses.
6. Click **Run test** to execute the runner.
7. Toggle between `view summmary` and `view results` for easy overview checkout.
8. Additional scripting can be used to change the request execution order.
   In the** Tests** tab for the previous **Get specific player** request, add the following code-

   `if(pm.response.json().data.played<750) postman.setNextRequest('Get all players');`

   The played number is a random int between 0-1000, so the code sets Postman up to re-run the collection from the **Get all players** request.
   However many times it takes to find a played value greater than 750.

9. With all of your requests saved, open the collection runner again, and click the run you ran earlier from the **Recent Runs** list.
10. Click **Run again** to run your collection again. It might run a different number of times whenever you run it depending on how long it takes to hit that 750 played value.

    > NOTE: The number of iterations for the runner can be explicitly set

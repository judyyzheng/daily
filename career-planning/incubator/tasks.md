## 1
The cats got their paws into the code and started building a pretty basic menu management app for their pizza joint. It’s not great...it’s not even good. They are master pizza makers, not app makers, and thus have enlisted the help of the human hands. Having a thumb helps a lot when typing. Your first objective is to get your environment set up and launch the app!

What You Will Learn:
- How to setup a development environment
- How to Utilize GIT and make PR’s to GitHub

##### 1.1
Install/update the following (if you haven’t already). See reference doc (Search for: Tools) for links, alternatives and configurations

- Visual Studio Code (you are free to use another IDE if you wish)
- Node and NPM
- GIT
- Docker & Docker Compose

## 2
Let’s start up the mongo database. So far there is only a list of basic toppings. Unfortunately, the cats aren’t happy with the current selection and have started knocking some of the topping jars of the kitchen table. If we don't satisfy them with some cat-friendly toppings quickly, there will soon be nothing left. Let’s add some kitty treats to the database and remove the toppings that are now spoiled on the floor.

What Done Looks Like:

- How to start up mongo database with docker
- How to CRUD (create, read, update, delete) directly on a MongoDB
- How a reference to Toppings is created for Pizzas

##### 2.1
The app is set up to compose a mongoDB database from a docker file for you. When you run docker and seed your database you should be able to see the default set of pizzas and toppings in robo 3t

Install all project dependencies

```yarn```

Create the MongoDB Database using Docker

```docker-compose up```

Run the seed script from the server route

```yarn seed:database```

Note: If you ever need to start up this container again use

```docker start <container-name>```

##### 2.2
While you were looking at the data, one of the cats ran off with your ball of yarn. Maybe the cat will come back if we update the toppings collection with some kitty treats and get rid of anything that attracts mice. Since we can’t start the app without yarn, we will have to mutate the database directly using robo-3t. Let's make some changes to the topping collection and add a cat-themed pizza for good measure.

Insert a topping into the Toppings collection ie: Insert a topping with ```name=”catnip”``` and ```priceCents=100```

Delete a topping from the Toppings collection  ie: Delete toppings with ```name=”swiss”```

Update a pizza from the Pizzas collection  ie:Lets rename the Margherita and add a few toppings ```name=”Meowgherita”```, ```description=”Margherita``` with a Samurai Twist”, and ```ToppingIds: [add some kitty treats]```

Screenshot your changes and comment them on your ticket before moving to “completed”
## 3
Now that the cats are distracted by the new toppings and pizza, we can grab the ball of yarn. Let’s run yarn start and see what they built so far. You should now be able to load the browser to navigate to the toppings page. It’s not pretty but it works. It’s cats, they can only type so fast!

Be sure to take a close look at the topping provider and resolver. The cats would like a way to manage their pizzas so the next step is to build out a pizza management page.

What you will learn:

- Creating a new React front end page
- Using Material UI components to build an interactive UI
- Building an API with Graph QL and MongoDB
- Querying and mutating data using apollo client
- You have tests for all the backend services

##### 3.1 
Before we can start showing Pizzas on the pizza page we need to build an API that resolves a pizzas query using a getPizzas provider method. 

Add a pizza provider that can get all pizzas with a method called getPizzas. This method should retrieve all the pizzas from the pizza collection as a mongo PizzaDocument interface (with ObjectID’s for the ID fields). Next, convert them to an array of Pizza interface (with strings for the ID fields). Make a helper function called toPizzaObject to handle the conversion (for reference: see toToppingObject)

Make a pizza schema with a Pizza type for the GraphQL resolver to use. The Pizza type will have the following fields: id, name, description, toppingIds, imgSrc. id should be an object id, and toppingIds will be an array of string’s for now 

Add a Query type to the pizza schema, it should have just one field, pizzas, and it should be of the Pizza type you just made.

Generate types every time you update a schema file

```yarn generate:types```

Add a pizza resolver that queries for pizzas using the getPizzas provider method you just created, this will return an array of objects with the Pizzas type (use the schema type that was generated here)

Test out your query in the Apollo GraphQL Sandbox and add a screenshot to your PR summary. It should match the one below

Note: If you are having trouble getting the query to work then you should double-check that your provider, resolver, and schema indexes are all up-to-date

##### 3.2

Now that we got a basic Pizza query working the next step is to query our toppings collection for toppings objects in place of their Id’s. We're going to add a field resolver for ```{{Pizza.toppings}}``` - In order to facilitate this, we need the current resolver to return an array of ids that we can use to look up the toppings in mongo.

Update the ```{{Pizza}}``` schema to return an array of ```{{Topping!}}``` instead of ToppingIds, then generate types. This is in preparation for step 3. 

```{{pizza.resolver.ts}}``` should have some typescript errors at this point since we updated the GraphQL schema types. Redefine the return type by omitting the schema's ```{{toppings}}``` field adding a new field called ```{{toppingIds}}```.

Update the Pizza interface returned by your provider, but keep the ```{{toppingIds}}``` field on the PizzaDocument type. In the toppings provider, create a ```{{getToppingsByIds}}``` method that accepts an array of topping ids and returns an array of toppings

In the topping resolver, resolve a toppings field as a child on its Pizza parent (instead of as a base query). In other words, Pizza is the parent and toppings will be the resolver. This task is in preparation for calculating the price of a pizza based on the price of toppings. The resolver method should look something like this:
```
Parent: {
  child: async (parent: { child }) => {
  // This function will talk to the children (toppings) 
  // provider and return an  array of them for each 
  // parent (pizza) }
}
```

Note: This function will talk to the children (toppings) provider and return an array of them for each parent. The Parent is the resolvers new Pizza type which has toppingIds and becomes the toppings field at this point. Refer to the Apollo GQL docs on Resolvers and Root/Trivial Resolvers if your having trouble understanding the parent/child relationship

Test out your query in the Apollo GraphQL Sandbox and add a screenshot to your PR summary. It should match the one below

##### 3.3
Now that your API is resolving a Pizza with an array of toppings, update the client to fetch each pizza with its toppings.

- Create a ```GET_PIZZAS``` query, you can copy and paste the one from the apollo sandbox
- Create a PizzaList react component that uses an apollo client useQuery hook for ```GET_PIZZAS```. 
- Restructure useQuery into loading, data, and error.
- When receiving an error you should display its message to the user.
- When loading, you should display the common CardItemSkeleton component, pass tit "pizza-list-loading" for its data-testid parameter. Don’t worry yet about making it look pretty, we will have tasks for that later.
- Make a PizzaItem component that displays each field you receive for a pizza inside the common CardItem component, when data is received you should map over it. 
- Use the pizza type from your generated schema for the data when you receive it, Then cast a new Pizza interface to the data when it's passed down to each PizzaItem as props.
- Display all pizzas on the pizza page inside a material-ui list.

##### 3.4
Now that we have our pizzas and their toppings showing up on the front end it would be nice to display each pizza's price. We want the price of a pizza to reflect the number of toppings it has. Instead of just summing the prices on the front end let's resolve it as a priceCents field with GraphQL.

Server:

- Add priceCents to the pizza schema and omit it in the pizza resolver.
- Create a getPriceCents method in the topping provider, it should return the summation of all the prices for all of its toppings. You can use reduce or sum the price together with a loop.
- Resolve price cents as a child field to pizza in the topping resolver using the provider method you just created (just like toppings).

Client:

- Add priceCents to your Pizza interface.
- Display it in your PizzaItem, use the toDollar helper method here to display a properly formatted price.


##### 3.5
Now that we have a list of Pizzas showing up on the Pizzas page, we need to make a few mutations so that the cats can manage the Pizza system by Creating, Updating, and Removing Pizzas.

Create:
- Add a mutation type with createPizza Mutation to the Pizza schema, you will need to also create an input for the mutation called CreatePizzaInput. The fields for the input should be name, description, imgSrc (a URL), and an array of toppingIds (the type is ObjectID’s). Don’t forget to generate types.
- Add a createPizza method to the Pizza’s provider, along with its interface. This method should call a mongo method on the Pizza collection to add a new pizza. This createPizza should insert the Pizza and return the promise of a Pizza (using toPizzaObject).
- Make sure all the inputs are valid! We don’t want empty strings or wonky fields being passed into our database. Use the validateStringInputs helper function and pass it an array of your string inputs.
- Make a topping provider method called validateToppings to check the toppings you are adding to pizzas exist, this method should return a void promise throw only throw errors when a topping can't be found.
- Add the accompanying resolver for the createPizza method, it should return the freshly created Pizza.
- Test your mutations manually using the Apollo GraphQL Sandbox and include a screenshot in your PR. They should use inputs similar to the one below.

Update:
- Similar to create but this time find and update an existing DB entry. Keep in mind that not all fields need to be changed for an update to be valid

Delete:
- Similar to the above but find and delete* an existing DB entry. Only the Id of the document you wish to delete is needed as an input here

Notes: Deleting documents entirely is typically not best practice, but it’s ok for this project. Ideally, you would set a deprecatedAt field on the document then clean out deprecated documents in set intervals.


##### 3.6

Now that we have the ability to mutate Pizzas on the backend we should test our providers and resolvers with jest. Test every provider and resolver method using the toppings backend tests as a reference.

Providers:
- Setup your testing suite by stubbing both the pizza collection and the topping provider. Instantiate a new pizza provider and pass both of your stubs into it.
- Create and use existing helper mock(s) for your provider inputs and outputs. You should use beforeEach to clear all the jest mocks so that one tests mock does not interfere with the next.
- Test the query, create, update and delete pizza methods in their own describe blocks.
- Each block should have its own beforeEach that utilizes the jest-auto-stub reveal method to mock the return of the mongo method used for this provider. 
- The return of the provider mocked you just need to have two tests, one to assert if the return value of the method is as expected and the second to if the appropriate calls are being made.

Resolvers:
- Setup your testing suite by mocking your database. This part of the setup is identical to the topping resolver. Don’t worry if you’re having trouble understanding why it is done this way, typically projects will have a repository layer that allows proper database mocking.
- You may want to create and use existing helper mock(s) for your resolver inputs and outputs.
- Use beforeAll to instantiate a new TestClient, you will need to pass it typeDefs and an array of the resolvers it uses.
- Test the query, and mutations in their own describe blocks. Similar to providers, assert the values you expect are returned and that the appropriate function calls are made.
- For each describe block you will need to define the query/mutation then apply it to the mocked TestClient using a query/mutate method.

##### 3.7
Now that the Pizzas API allows mutations we need to access and trigger them from the client side. Write out mutations and use them in a paw-friendly UI so the cats can edit their menu easily. Cats love playing with things that are interactive so let’s make a pop-up modal using material-UI.

Create:
- Similar to how you defined your pizza query, define a CREATE_PIZZA mutation inside a new mutation folder and export it to an index. Take a look at the mutation in your apollo sandbox if you need a hand, you’ll need to define an input for this and the syntax is a bit tricky. The input does not need to be imported for this (I know it’s weird).
- Make a custom hook handler called usePizzaMutations. In this hook, create a method called onCreate that makes a request to the server through Apollo’s useMutation hook. This mutation should use a locally defined input called onCreateInput.
- Define a selectPizza function in your Pizza component that assigns a pizza to a selectedPizza state with useState then pass this function down to each pizza item.
- Add an onClick event listener to the PizzaItem component that calls a selectPizza function and sets the state to the currently selected pizza.
- Create a PizzaModal component using the material UI modal as a starting point.
- Each time SelectPizza is called this modal should open, use state again here to handle if the modal is open.
- The modal should contain a Formik Form that uses Formik fields for each input. Toppings can be Added/Removed using a Material-UI component of your choice (Checkbox list, dropdown list, etc).
- On submit, the onCreate hook should be called, the modal should close and the new Pizza should appear in the list. If a new pizza isn’t showing up you should look into the refetchQueries input for graphQL’s useMutation hook. Depending on how you set things up, useCallback may have to be used here to monitor if the pizza list has any changes.

Update:
- Add an onUpdate function in the usePizzaMutations hooks. This will be very similar to onCreate.
- It’s good practice to make modular components, use the same modal as onCreate for onUpdate. Your Formik Form should pre-populate with the current values of the selected pizza.

Delete: 
- Add a delete button to the Pizza modal, that when clicked, will delete the Pizza.
- The mutation call should follow a similar pattern to add and update.


##### 3.8
Now that the front end is fully built out, lets test the Pizza list and its items.

Pizza List: 
- Create a describe block for PizzaList and set up your suite by rendering a PizzaList with the renderWithProviders testing method (it’s already in your lib/test folder).
- Set Up a mockPizzaQuery using msw-server’s server.use method, pass this method two test pizzas.
- Test that the list of pizzas is visible on the page as expected using waitFor, then expect that the screen can findAllByTestId for a data-testid field on each PizzaItem. If you PizzaItems don’t have this id then you should add it. This test should check that it resolves with a toHaveLength of 2.
- Test the loading case as well (as we wait for our query to resolve, test what the user sees is as expected). You should use waitFor and check that queryByTestId is not null and visible on the screen.

Pizza Item: 
- Create a describe block for PizzaItem and set up your suite by rendering a PizzaItem with the renderWithProviders testing method again.
- You should pass this method props so it can generate a selected mockPizza.
- Test the item is visible and has the correct components with toContain (e.g. is name, description etc …  visible?).
- Test the behaviour of clicking on the list, by again rendering a PizzaItem then trigger a userEvent and verify that the Pizza was selected by checking to see if the selectPizzafunction was called once with toHaveBeenCalledTimes.

## 4
Cats vision is excellent, a little too excellent. Now that they’ve used the site for a bit, they realize, it’s pretty atrocious. It’s time to clean this site up so it’s presentable and clear.

What you will learn:
- Responsive Design
- How to use Material-UI Cards and grids
- Pagination with cursors

##### 4.1
At this point, your Pizza list should be a simple list. Let’s update it with cards so that the cats can see a nice big picture for each pizza instead of a tiny icon inside a list row. The cats also like playing with their window sizes which might mess up the UI so let's make sure it's responsive.

The Pizzas page should be displaying a list of Pizzas, put them into a Material-UI Grid that is 3 tiles wide, and turn each list item into a Material-UI Card.

You will need to update the UI to make it look clean. Each tile should function as an edit button that opens a modal with a submit and delete button inside of it.

Ensure that the cards are responsive (ie. if you shrink the width of the page they stack properly).


##### 4.2
The Cats have been going crazy and now they’ve added 100’s of pizzas. This is slowing down the browser page for pizzas and overwhelming the cats. In this ticket, we will refactor the “Pizzas” resolver to use cursor-based pagination. Cursors grab documents from a collection in sections, each section has a limit for the max number of documents it can hold and a cursor for the last item it collects. The cursor field can continue to be used to fetch more documents until there are none left.

Create a new Provider for your cursor, it should have a method called getCursorIndex that keeps track of the current position of a cursor by counting the index to the last cursors document id.

Make a second method called getCursorResult which accepts a limit and cursor argument (it is up to you if you want to implement other arguments like sort and filter). The method should call getCursorIndex to find out how many items it should skip, then use its parameters to query the database and return the next set of results. This method should return a boolean for a field hasNextPage, string ID/null for the current position of the cursor, the totalCount of documents, and a results field that contains the Pizzas.

Import the CursorProvider into your PizzaProvider and update getPizzas to use getCursorResults, you will have to make a new return interface called GetPizzasResponse. You will also need to make a cursor schema and update the Pizza schema to include the cursor fields.

Check Test your mutations manually using the Apollo GraphQL Sandbox to make sure you can query pizzas with a cursor (include a screenshot in your PR).

Update all your tests that pertain to the Pizzas API so that they still pass.


##### 4.3
Now that the API supports cursor queries we need to update the client to request and display paginated results.

Update the GraphQL query to include the fields you added for pagination and the input to trigger it (you can copy this from the sandbox). It is up to you how you want to implement this (e.g. if you want to do pages or if you simply want to have more pizzas that load every time you scroll down).

Update your tests to work with the new query.

Add additional tests to verify that pagination is triggered when the user either scrolls to the bottom of the page or selects a button to load more results.


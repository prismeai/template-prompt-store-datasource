## 0. Export This Repository

- **Quick method**: Download this repository as a `.zip` file and import or drag & drop it onto the **AI Builder Home**.  
- **Git method**: Clone this repository, create a workspace in AI Builder, and link your repository to the workspace.  
👉 Refer to the [AI Builder Versioning Documentation](https://docs.prisme.ai/products/ai-builder/deployment) for more information.

## 2. Retreive your endpoint

Go to your new fresh Workspace in the AI Builder and look into the "endpoint" automation. Find the green Trigger bubble and click the "URL" button to copy the endpoint.

## 3. Configure your AI Knowledge agent datasource

- Go to AI Knowledge and choose your agent. Navigate to Advanced > Datasources and click on "Add a new Datasource"
- Fill your datasource title and description, and finaly paste your endpoint in the field "Options endpoint URL".
- Save

## 4. Find your new Datasource in AI Store

Go to AI Store, talk to your agent. You will find a "+" button on the left side of the chat input. Click it and you'll find your datasource listed in the menu. Click it and your prompts will be displayed in a right sidebar.

# Advanced development

At first, your prompt store is a simple static fixtures list. You need to write the getPrompts automation to define where you will store and retreive your prompts.

## Storing prompts

### Statically from automation

The simplest way is to set your prompts statically in the getPrompts automation. Respect the following structure for your items :

```ts
interface Item {
  icon: 'active' | 'inactive' | string; // You can define an image as icon but you can juste keep 'active' as default
  title: {
    fr: string
    en: string
  }; // The item title, localized
  description:{
    fr: string
    en: string
  }; // The item description, localized
  author?: string; // The author name, optional
  updatedAt: string; // The last update date as ISO string
  value: {
    input: string; // The more important : the prompt which will be set in user input
  };
  id: string; // A unique id. If you want to use the counter functionnality, keep it stable
}
```

### Dynamically from Collection

You may want to store your prompt in a collection if you have too much to be store in a simple automation. Install the Collection App in your Worlspace, rename it "Prompts" and start using it to insert, update and find documents in the `getPrompts` automation.

### Dynamically from web API

Replace the content of `getPrompts` by a `fetch` instruction to make an API call or just one of our available Apps like Github to fetch your prompts in an external store.

## Paginating

The endpoint automation will output the following structure:

```ts
interface Output {
  items: Item[]; // Array of items of
  total: number; // Total count of items
  page: number; // Current page
  perPage: number; // Items count per page
  onSelect: {
    url?: string;
  }; // Optional callback endpoint called when user select an item.
  selection: 'single'; // In the use case of a prompt picker, the selection must be single.
}
```

## Increment counter

You can see in the output of endpoint automation a `onSelect` attribute with an url pointing to the `increment` automation. This endpoint will be called everytime a user select an item. In this template, it increments a counter in a collection, which is used as "usage counter". But you can implement your own logic or disable it by removing the `onSelect` attribute from the output.

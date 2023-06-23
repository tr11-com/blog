---
title: "Supabase: What is it & Getting started"
datePublished: Wed Jun 07 2023 09:42:17 GMT+0000 (Coordinated Universal Time)
cuid: clj8dvlwa001709mj48yj19el
slug: supabase-what-is-it-getting-started
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/GNyjCePVRs8/upload/fff03efbafd61cb7e9dd6db6eabe7353.jpeg
tags: tools, supabase

---

## What is Supabase?

Supabase is an open-source Firebase alternative that aims to provide developers with a Firebase-like experience using an enterprise-grade platform. It combines a suite of open-source tools, emphasizing the use of existing tools and communities with MIT, Apache 2, or equivalent open licenses. If an appropriate tool doesn't exist, the team behind Supabase develops and open sources it themselves.

Supabase is a hosted platform, meaning you can sign up and start using it without needing to install anything on your local machine. However, if you prefer, you can self-host and develop locally.

## Key Features of Supabase

Supabase offers a range of features that aim to provide a comprehensive backend solution for developers:

* **Database:** Supabase provides a full Postgres database for every project. It supports real-time functionality, database backups, extensions, and more.
    
* **Authentication and Authorization:** Supabase offers a suite of identity providers and APIs that allow you to add and manage various types of logins to your project, including email and password, passwordless, OAuth, and mobile logins.
    
* **Auto-generated APIs:** Supabase auto-generates REST and GraphQL APIs, enabling real-time subscriptions.
    
* **Functions:** Supabase supports server-side functions, including database functions and edge functions, which execute your code closest to your users for the lowest latency.
    
* **File Storage:** With Supabase, you can store, organize, transform, and serve large files. The storage is fully integrated with your Postgres database and supports Row Level Security access policies.
    
* **AI & Vectors:** Supabase allows you to store and search embedding vectors, a feature that comes in handy when working with AI-based applications.
    
* **Realtime Listening:** This feature enables you to listen to database changes, store, and sync user states across clients, broadcast data to clients subscribed to a channel, and more.
    

The philosophy behind Supabase's approach to client libraries is modular, meaning that each sub-library is a standalone implementation for a single external system.

Supabase supports a range of client languages, including JavaScript (and TypeScript), Flutter, C#, Go, Java, Kotlin, Python, Ruby, Rust, Swift, and even the Godot Engine (GDScript).

## Getting Started with Supabase

Now that we've covered what Supabase is and its key features, let's walk through how to get started with a simple project using Supabase and React.

### Setting up a Supabase Project

First, you'll need to create a new project in the Supabase Dashboard. After your project is ready, you'll create a table in your Supabase database using the SQL Editor in the Dashboard. As an example, we'll create a `countries` table with some sample data using the following SQL statement:

```sql
CREATE TABLE countries (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL 
);

INSERT INTO countries (name) VALUES ('United States');
INSERT INTO countries (name) VALUES ('Canada');
INSERT INTO countries (name) VALUES ('Mexico');
```

This statement will create a new table called "countries" with two columns: "id" and "name". Then, it inserts the names of three countries into the table.

### Creating a React App

Next, create a React app using a Vite template. You can do this by running the following command in your terminal:

```bash
npm create vite@latest my-app -- --template react
```

This command creates a new React application using Vite, a modern front-end build tool, with the project name "my-app".

### Installing the Supabase Client Library

The Supabase client library, `supabase-js`, provides a convenient interface for working with Supabase from a React app. Navigate to your newly created React app and install `supabase-js` by running the following command:

```bash
cd my-app && npm install @supabase/supabase-js
```

This command navigates into your "my-app" directory and installs the `supabase-js` library.

### Querying Data from the App

Now, we'll set up a way to query data from the Supabase database and display it in your app. In `App.jsx`, create a Supabase client using your Project URL and public API (anon) key. Then, add a `getCountries` function to fetch the data from the `countries` table we created earlier and display the query result to the page. Here is a simple implementation:

```jsx
import { useEffect, useState } from "react";
import { createClient } from "@supabase/supabase-js";

const supabase = createClient("https://<project>.supabase.co", "<your-anon-key>");

function App() {
    const [countries, setCountries] = useState([]);

    useEffect(() => {
        getCountries();
    }, []);

    async function getCountries() {
        const { data } = await supabase.from("countries").select();
        setCountries(data);
    }

    return (
        <ul>
            {countries.map((country) => (
                <li key={country.name}>{country.name}</li>
            ))}
        </ul>
    );
}

export default App;
```

This code creates a new Supabase client with your project URL and public API key, fetches the country data from the database, and displays it in an unordered list.

### Starting the App

Finally, start your app by running the following command in your terminal:

```bash
npm run dev
```

Then, navigate to [http://localhost:5173](http://localhost:5173) in a web browser, and you should see the list of countries displayed on the page.

## Wrapping Up

Supabase is a powerful open-source alternative to Firebase that offers a suite of features for developing web and mobile applications. It's built on top of a stack of open-source tools and is designed to give developers a Firebase-like experience. Whether you're looking for a real-time database, authentication services, auto-generated APIs, or file storage capabilities, Supabase has you covered

Moreover, it supports a variety of languages and platforms, making it a versatile choice for many developers. The step-by-step guide above will help you get started with your first Supabase project, providing a basic understanding of how to create a project, add data to your database, and query that data from a React app. Happy coding!
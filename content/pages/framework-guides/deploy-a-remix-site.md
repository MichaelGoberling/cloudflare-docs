---
pcx_content_type: how-to
title: Remix
---

# Remix

[Remix](https://remix.run/) is a framework that is focused on fully utilizing the power of the web. Like Cloudflare Workers, it uses modern JavaScript APIs, and it places emphasis on web fundamentals such as meaningful HTTP status codes, caching and optimizing for both usability and performance.

In this guide, you will create a new Remix application and deploy to Cloudflare Pages.

## Setting up a new project

Use the [`create-cloudflare`](https://www.npmjs.com/package/create-cloudflare) CLI (C3) to set up a new project. C3 will create a new project directory, initiate Remix's official setup tool, and provide the option to deploy instantly.

To use `create-cloudflare` to create a new Remix project, run the following command:

```sh
$ npm create cloudflare@latest my-remix-app -- --framework=remix
```

`create-cloudflare` will install additional dependencies, including the [Wrangler](/workers/wrangler/install-and-update/#check-your-wrangler-version) CLI and any necessary adapters, and ask you setup questions.

{{<Aside type="warning" header="Before you deploy">}}
Your Remix project will include a `functions/[[path]].ts` file. The `[[path]]` filename indicates that this file will handle requests to all incoming URLs. Refer to [Path segments](/pages/functions/routing/#dynamic-routes) to learn more.

The `functions/[[path]].ts` will not function as expected if you attempt to deploy your site before running `remix vite:build`.
{{</Aside>}}

After setting up your project, change the directory and render your project by running the following command:

```sh
# choose Cloudflare Pages
$ cd my-remix-app
$ npm run dev
```

{{<render file="_tutorials-before-you-start.md">}}

{{<render file="/_framework-guides/_create-github-repository_no_init.md">}}

## Deploy with Cloudflare Pages

{{<render file="_deploy-via-c3.md" withParameters="Remix">}}

### Deploy via the Cloudflare dashboard

1. Log in to the [Cloudflare dashboard](https://dash.cloudflare.com/) and select your account.
2. In Account Home, select **Workers & Pages** > **Create application** > **Pages** > **Connect to Git**.
3. Select the new GitHub repository that you created and, in the **Set up builds and deployments** section, provide the following information:

{{<pages-build-preset framework="remix">}}

After configuring your site, you can begin your first deploy. You should see Cloudflare Pages installing `npm`, your project dependencies, and building your site before deploying it.

{{<Aside type="note">}}

For the complete guide to deploying your first site to Cloudflare Pages, refer to the [Get started guide](/pages/get-started/).

{{</Aside>}}

After deploying your site, you will receive a unique subdomain for your project on `*.pages.dev`.
Every time you commit new code to your Remix site, Cloudflare Pages will automatically rebuild your project and deploy it. You will also get access to [preview deployments](/pages/configuration/preview-deployments/) on new pull requests, so you can preview how changes look to your site before deploying them to production.

### Deploy via the Wrangler CLI

If you use [`create-cloudflare`(C3)](https://www.npmjs.com/package/create-cloudflare) to create your new Remix project, C3 will automatically scaffold your project with [`wrangler`](/workers/wrangler/). To deploy your project, run the following command:

```sh
$ npm run deploy
```

## Create and add a binding to your Remix application

To add a binding to your Remix application, refer to [Bindings](/pages/functions/bindings/).
A [binding](/pages/functions/bindings/) allows your application to interact with Cloudflare developer products, such as [KV namespaces](/kv/concepts/how-kv-works/), [Durable Objects](/durable-objects/), [R2 storage buckets](/r2/), and [D1 databases](/d1/).

### Binding resources in local development

Remix uses Wrangler's [`getPlatformProxy`](/workers/wrangler/api/#getplatformproxy) to simulate the Cloudflare environment locally. You configure `getPlatformProxy` in your project's `vite.config.ts` file via [`cloudflareDevProxyVitePlugin`](https://remix.run/docs/en/main/future/vite#cloudflare-proxy).

To bind resources in local development, you need to configure the bindings in the `wrangler.toml` file. Refer to [Bindings](/workers/wrangler/configuration/#bindings) to learn more.

Once you have configured the bindings in the `wrangler.toml` file, the proxies are then available within `context.cloudflare` in your `loader` or `action` functions:

```typescript
export const loader = ({ context }: LoaderFunctionArgs) => {
  const { env, cf, ctx } = context.cloudflare;
  env.MY_BINDING // Access bound resources here
  // ... more loader code here...
};
```

{{<Aside header="Correcting the env type">}}
You may have noticed that `context.cloudflare.env` is not typed correctly when you add additional bindings in `wrangler.toml`.

To fix this, run `npm run typegen` to generate the missing types. This will update the `Env` interface defined in `worker-configuration.d.ts`.
After running the command, you can access the bindings in your `loader` or `action` using `context.cloudflare.env` as shown above.
{{</Aside>}}


### Binding resources in production

To bind resources in production, you need to configure the bindings in the Cloudflare dashboard. Refer to the [Bindings](/pages/functions/bindings/) documentation to learn more.

Once you have configured the bindings in the Cloudflare dashboard, the proxies are then available within `context.cloudflare.env` in your `loader` or `action` functions as shown [above](#binding-resources-in-local-development).

## Example: Access your D1 database in a Remix application

As an example, you will bind and query a D1 database in a Remix application.

1. Create a D1 database. Refer to the [D1 documentation](/d1/) to learn more.
2. Configure bindings for your D1 database in the `wrangler.toml` file:

```toml
[[ d1_databases ]]
binding = "DB"
database_name = "<YOUR_DATABASE_NAME>"
database_id = "<YOUR_DATABASE_ID>"
```
3. Run `npm run typegen` to generate TypeScript types for your bindings.

```sh
$ npm run typegen
> typegen
> wrangler types

 ⛅️ wrangler 3.48.0
-------------------
interface Env {
	DB: D1Database;
}
```

4. Access the D1 database in your `loader` function:

```typescript
---
filename: app/routes/products/$productId.tsx
---
import type { LoaderFunction } from "@remix-run/cloudflare";
import { json } from "@remix-run/cloudflare";
import { useLoaderData } from "@remix-run/react";

export const loader: LoaderFunction = async ({ context, params }) => {
  const { env, cf, ctx } = context.cloudflare;
  let { results } = await env.DB.prepare(
    "SELECT * FROM products where id = ?1"
  ).bind(params.productId).all();
  return json(results);
};

export default function Index() {
  const results = useLoaderData<typeof loader>();
  return (
    <div>
      <h1>Welcome to Remix</h1>
      <div>
        A value from D1:
        <pre>{JSON.stringify(results)}</pre>
      </div>
    </div>
  );
}
```

{{<render file="/_framework-guides/_learn-more.md" withParameters="Remix">}}
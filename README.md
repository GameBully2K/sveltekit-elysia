# SvelteKit Lucia Project Template

Welcome to your SvelteKit Lucia project template! This repository provides a basic setup for building applications using SvelteKit, complete with user authentication and database integration.

**Svelte-kit**: Typescript-ready Meta-framework.

**Lucia Auth**: low abstraction Auth Library.

**Drizzle ORM**: lite weight Object Relational Mapper.

**Cloudflare Stack**: Cloudflare d1 SQL database, KV namespace, and more through the wrangler interface.

**Tailwind CSS**: CSS framework for rapid UI development.

**SKELETON UI**: Famous Svelte library.


## Getting Started

Follow these steps to set up your project:

### 0. As usual
```bash
npm i
```

### 1. Create a D1 Database
Set up a D1 database to store your application data.
```bash
wrangler d1 create sveltekit-lucia
```
and add the output toml bindings to your wrangler.toml file:
```toml
[[d1_databases]]
binding = "DB" # i.e. available in your Worker on env.DB
database_name = "sveltekit-luciadb"
```

### 2. Create a KV Namespace
Create a KV namespace for managing key-value storage.
```bash
wrangler kv:namespace create sveltekit-lucia
```
and add the output toml bindings to your wrangler.toml file:
```toml
[[kv_namespaces]]
binding = "sveltekit-luciakv"
id = "da88841132724cf8a41416d92a476e95"
```

### 3. Configure `wrangler.toml`
Ensure that your `wrangler.toml` file looks something like this:

```toml
name = "sveltekit-elysia"
compatibility_date = "2024-06-20"
pages_build_output_dir = ".svelte-kit/cloudflare"

[vars]
MY_VARIABLE = "production_value"

[[d1_databases]]
binding = "DB" # i.e. available in your Worker on env.DB
database_name = "sveltekit-luciadb"

[[kv_namespaces]]
binding = "sveltekit-luciakv"
id = "da88841132724cf8a41416d92a476e95"
```

#### NB:
##### Run cf-typegen command to update the env variables
```bash
npm run cf-typegen
```
##### Change the kvnamespace name in the whole directorie with
```bash
SHIFT +  ALT + F
```

### 4. Apply Default Migrations
Run the default migration to create the necessary tables for users and sessions. This can be done both remotely and locally.

##### Run migrations on production
```bash
npx wrangler d1 migrations apply <dbname> 
```
##### Run migrations localy
```bash
npx wrangler d1 migrations apply <dbname>  --local
```

### 5. Create and Add ENV Credentials
To enable user login functionality, you need to add either a Brevo SMTP key or Google OAuth credentials (or both) in your .dev.vars file

- **Brevo SMTP Key**: Follow Brevo's documentation to generate an SMTP key.

    Also don't forget to edit the content of the email and the sender address in ./src/lib/emailing/brevo.ts 
- **Google OAuth Credentials**: Create OAuth credentials in the Google Developer Console.

Make sure you .dev.vars file looks like this:
```env
SMTP_API_KEY=xkeysib-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

GOOGLE_CLIENT_ID=0000000-xxxxxxxxxxxxxxxxxxxxxx.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=xxxxxxxxxxxxxxxxxxxxxxx
```

### 6. Accessing User Objects
Once set up, you will have access to the user's object globally in every request event or page load event.

### 7. Drizzle Kit
If you edit your schema and want to update your db you can use:
```bash
npx drizzle-kit generate
```

## Project Structure

- `src/`: Contains your Svelte components and application logic.
- `static/`: Static assets like images, fonts, etc.
- `migrations/`: Database migration files.
- `wrangler.toml`: Configuration file for Cloudflare Workers.

## License

This project is open source and available under the [MIT License](LICENSE).

## Acknowledgments

- [SvelteKit](https://kit.svelte.dev/)
- [Cloudflare Workers](https://workers.cloudflare.com/)
- [Brevo](https://www.brevo.com/)

Feel free to modify any sections to better fit your project's specifics!

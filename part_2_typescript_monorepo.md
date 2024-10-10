# Part 2: Typescript Monorepo

## Yarn Workspaces

- You can achieve a simple setup with Yarn Workspaces
- You can depend on other workspaces in the monorepo, by adding the depency in the package.json with the `workspace:` prefix and running yarn install
- With newer yarn versions, you might need to use `nodeLinker: node-modules`. Using the default option, `nodeLinker: node-modules` didn't always symlink the workspaces correctly in the past for me

## More Advanced Setup with Turbo and PNPM

- https://www.youtube.com/watch?v=hRyU0bN7qhw

### Examples of Typescript Monorepos

- Runs in the browser: https://github.com/drizzle-team/drizzle-orm
- Contains apps that run in Node.js: https://github.com/mattpocock/total-typescript-monorepo

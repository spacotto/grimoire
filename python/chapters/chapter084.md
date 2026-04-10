# Environment Variables

Environment variables are **key-value pairs** that live outside your code and are used to pass **configuration and secrets to your application at runtime**. This doc covers how to read, set, and manage them in Python 3, along with best practices for security and portability. 

## What are Environment Variables?

Environment variables are named values stored in the process environment, outside of your application's source code. They're set by the shell, the OS, or a deployment platform, and inherited by child processes. Common uses are API keys, database URLs, feature flags, and application mode (DEBUG, PRODUCTION).

```shell
# Shell — setting before running a script
DATABASE_URL=postgres://localhost/mydb python app.py
```

## Reading Environment Variables (os.environ)

## Setting Environment Variables

## Environment Variables in Different OS

## Environment Variable Naming Conventions

## When to Use Environment Variables

## Security with Environment Variables

## Environment Variable Precedence

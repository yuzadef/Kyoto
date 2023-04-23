# Redis

## Summary
- [Theory](#theory)
- [Basic usage](#basic-usage)
  - [Connecting to the service](#connecting-to-the-service)
  - [Useful commands](#useful-commands)

## Theory
Redis is an open source, in-memory, NoSQL key/value store that is used primarily as an application cache or quick-response database.

## Basic usage
### Connecting to the service
```
redis-cli -h 10.0.0.1 -a "password"
netcat 10.0.0.1 6379
```

### Useful commands
| Commands | Description |
| -------- | ----------- |
| keys * | Retrieve all keys |
| keys "share name" | Retrieve a share |
| get "share name" | Download a share |
| lrange "share name" 1 100 | List a share's value |

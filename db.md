# Database

## Redis

https://redis.io/

### Flushing keys with little blocking

- [How to delete keys matching a pattern in Redis](https://rdbtools.com/blog/redis-delete-keys-matching-pattern-using-scan/): Use `scan` over `keys` and `unlink` over `del` for better performance
- [FLUSHALL ASYNC](https://redis.io/commands/flushall) to flush all keys

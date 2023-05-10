# Static Analysis Process


## Sanitation Process
- Rector
- ECS
- PHPStan

## Commands
### Rector
```bash
# Check
vendor/bin/rector process --dry-run
```

```bash
# Apply Rector
vendor/bin/rector process
```

### ECS

```bash
# Check
vendor/bin/ecs check path/to/folder
```
```bash
# Apply fix
vendor/bin/ecs check path/to/folder --fix
```

## DOC Block Generator
```php
php artisan ide-helper:models "App\Models\UsesUuid"
```
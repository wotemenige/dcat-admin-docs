# 关联关系

## 一对一
`users`表和上面的`posts`表为一对一关联关系，通过`posts.author_id`字段关联，`users`表结构如下：

```sql
CREATE TABLE `users` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(255) COLLATE utf8_unicode_ci NOT NULL,
  `email` varchar(255) COLLATE utf8_unicode_ci NOT NULL,
  `created_at` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00',
  `updated_at` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;
```
模型定义为：

```php
class User extends Model
{
}

class Post extends Model
{
    public function author()
    {
        return $this->belongsTo(User::class, 'author_id');
    }
}
```
数据仓库定义为：
```php
<?php

namespace App\Admin\Repositories;

use Dcat\Admin\Repositories\EloquentRepository;
use User as UserModel;

class User extends EloquentRepository
{
    protected $eloquentClass = UserModel::class;
}
```

那么可以用下面的方式显示post所属的用户的详细：

```php
use App\Admin\Repositories\User;

$show->author(function ($model) {
    $show = new Show(new User);
    
    $show->setId($model->author_id);
    $show->resource('/users');

    $show->id();
    $show->name();
    $show->email();
    
    return $show;
});
```

注意：为了能够正常使用这个面板右上角的工具，必须用`resource`方法设置用户资源的url访问路径

## 一对多
一对多会以[数据表格](model-grid.md)的方式呈现，下面是简单的例子

`posts`表和评论表`comments`为一对多关系(一条post有多条comments)，通过`comments.post_id`字段关联

```sql
CREATE TABLE `comments` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `post_id` int(10) unsigned NOT NULL,
  `content` varchar(255) COLLATE utf8_unicode_ci NOT NULL,
  `created_at` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00',
  `updated_at` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci
```
模型定义为：

```php
class Post extends Model
{
    public function comments()
    {
        return $this->hasMany(Comment::class);
    }
}

class Comment extends Model
{
}
```
数据仓库定义为：
```php
<?php

namespace App\Admin\Repositories;

use Dcat\Admin\Repositories\EloquentRepository;
use Comment as CommentModel;

class Comment extends EloquentRepository
{
    protected $eloquentClass = CommentModel::class;
}
```

那么评论的显示通过下面的代码实现：

```php
use App\Admin\Repositories\Comment;

$show->comments(function ($model) {
    $grid = new Grid(new Comment);
    
    $grid->model()->where('post_id', $model->id);
    
    $grid->resource('/admin/comments');

    $grid->id();
    $grid->content()->limit(10);
    $grid->created_at();
    $grid->updated_at();

    $grid->filter(function ($filter) {
        $filter->like('content')->width('300px');
    });
    
    return $grid;
});
```

注意：为了能够正常使用这个数据表格的功能，必须用`resource()`方法设置`comments`资源的url访问路径

## 多对多

多对多会以[数据表格](model-grid.md)的方式呈现，下面是简单的例子

角色表`roles`和权限表`permissions`为多对多关系，通过中间表`role_permissions`关联

```sql
CREATE TABLE `roles` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(50) COLLATE utf8mb4_unicode_ci NOT NULL,
  `slug` varchar(50) COLLATE utf8mb4_unicode_ci NOT NULL,
  `created_at` timestamp NULL DEFAULT NULL,
  `updated_at` timestamp NULL DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `admin_roles_name_unique` (`name`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

CREATE TABLE `permissions` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(50) COLLATE utf8mb4_unicode_ci NOT NULL,
  `slug` varchar(50) COLLATE utf8mb4_unicode_ci NOT NULL,
  `http_method` varchar(255) COLLATE utf8mb4_unicode_ci DEFAULT NULL,
  `http_path` text COLLATE utf8mb4_unicode_ci,
  `order` int(11) NOT NULL DEFAULT '0',
  `parent_id` int(11) NOT NULL DEFAULT '0',
  `created_at` timestamp NULL DEFAULT NULL,
  `updated_at` timestamp NULL DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `admin_permissions_name_unique` (`name`)
) ENGINE=InnoDB AUTO_INCREMENT=9 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

CREATE TABLE `role_permissions` (
  `role_id` int(11) NOT NULL,
  `permission_id` int(11) NOT NULL,
  `created_at` timestamp NULL DEFAULT NULL,
  `updated_at` timestamp NULL DEFAULT NULL,
  KEY `admin_role_permissions_role_id_permission_id_index` (`role_id`,`permission_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```
模型定义为：

```php
class Role extends Model
{
  public function permissions() : BelongsToMany
  {
      return $this->belongsToMany(Permission::class, 'role_permissions', 'role_id', 'permission_id');
  }

}

class Permission extends Model
{
}
```
数据仓库定义为：
```php
<?php

namespace App\Admin\Repositories;

use Dcat\Admin\Repositories\EloquentRepository;
use Permission as PermissionModel;

class Permission extends EloquentRepository
{
    protected $eloquentClass = PermissionModel::class;
}
```

那么权限的显示通过下面的代码实现：

```php
use App\Admin\Repositories\Permission;

$show->permissions(function ($model) {
    $grid = new Grid(new Permission);

    $grid->model()->join('role_permissions', function ($join) use ($model) {
        $join->on('role_permissions.permission_id', 'id')
            ->where('role_id', '=', $model->id);
    });

    $grid->resource('auth/permissions');

    $grid->id;
    $grid->name;
    $grid->slug;
    $grid->http_path;

    $grid->filter(function (Grid\Filter $filter) {
        $filter->equal('id')->width('300px');
    });

    return $grid;
});
```

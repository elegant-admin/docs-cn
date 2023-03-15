# 快速开始

## 数据表结构和模型 {#数据表结构和模型}

用`Laravel`自带的`users`表举例,表结构为：

```sql
users
    id          - integer
    name        - string
    email       - string
    password    - string
    created_at  - timestamp
    updated_at  - timestamp
```

对应的数据模型为文件 `App\User.php`

使用`laravel-admin`可以通过使用以下几步来快速生成`users`表的`CURD`操作页面：

## 创建控制器 {#创建控制器}

使用下面的命令来创建一个`App\User`模型对应的控制器

```shell
// Mac os、 Linux
php artisan admin:make UserController --model=App\\User

// Windows
php artisan admin:make UserController --model=App\User
```

在`v1.8.0`版本可以使用`admin:controller`命令创建控制器：

```shell
php artisan admin:controller --model=App\User
```

上面的命令会创建控制器文件`app/Admin/Controllers/UserController.php`.

## 添加路由 {#添加路由}

在路由配置文件`app/Admin/routes.php`里添加一行：

```php
$router->resource('users', UserController::class);
```

## 添加菜单栏入口 {#添加菜单栏入口}

打开菜单管理页`http://localhost:8000/admin/auth/menu`，添加对应的menu, 然后就能在后台管理页面的左侧边栏看到用户管理页面的链接入口了。

> 其中`uri`填写不包含路由前缀的的路径部分，比如完整路径是`http://localhost:8000/admin/demo/users`, 那么就填`demo/users`
>
> 如果要添加外部链接，只要填写完整的url即可，比如`http://laravel-admin.org/`.

## 编写CURD页面逻辑 {#编写CURD页面逻辑}

通过`admin:make`命令创建的控制器`app/Admin/Controllers/UserController.php`如下：

```php
<?php

namespace App\Admin\Controllers;

use App\Models\User;
use Encore\Admin\Controllers\AdminController;
use Encore\Admin\Form;
use Encore\Admin\Grid;
use Encore\Admin\Show;

class UserController extends AdminController
{
    protected $title = 'Users';

    protected function grid()
    {
        $grid = new Grid(new User());

        $grid->column('id', __('Id'));
        $grid->column('name', __('Name'));
        $grid->column('email', __('Email'));
        $grid->column('password', __('Password'));
        $grid->column('created_at', __('Created at'));
        $grid->column('updated_at', __('Updated at'));

        return $grid;
    }

    protected function detail($id)
    {
        $show = new Show(User::findOrFail($id));

        $show->field('id', __('Id'));
        $show->field('name', __('Name'));
        $show->field('email', __('Email'));
        $show->field('password', __('Password'));
        $show->field('created_at', __('Created at'));
        $show->field('updated_at', __('Updated at'));

        return $show;
    }

    protected function form()
    {
        $form = new Form(new User());

        $form->textarea('name', __('Name'));
        $form->textarea('email', __('Email'));
        $form->textarea('password', __('Password'));

        return $form;
    }
}
```

`$title`属性用来设置这个CURD模块的标题，可以将它修改为任何其它的字符串。

`grid`方法对应数据的`列表`页，参考[model-grid 文档](https://laravel-admin.org/docs/zh/1.x/model-grid.md)来实现列表页的相关功能逻辑。

`detail`方法对应数据的`详情`页，在列表页操作列的`详情显示`按钮点击进入，参考[model-show 文档](https://laravel-admin.org/docs/zh/1.x/model-show.md)来实现详情页的相关功能逻辑。

`form`方法对应数据的`创建`和`编辑`页，参考[model-form 文档](https://laravel-admin.org/docs/zh/1.x/model-form.md)来实现数据创建和编辑页的相关功能逻辑。
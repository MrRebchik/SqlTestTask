<h2>SqlTestTask</h2>
<h4>Задание:</h4>
<p>
  В базе данных MS SQL Server есть продукты и категории. Одному продукту может соответствовать много категорий, в одной категории может быть много продуктов.
  Напишите SQL запрос для выбора всех пар «Имя продукта – Имя категории». Если у продукта нет категорий, то его имя все равно должно выводиться.
</p>
<h4>Решение:</h4>
<p>
  Создание базы данных и таблиц.
</p>

```
CREATE DATABASE storedb;
GO

USE storedb;
CREATE TABLE Products
(
    Id INT PRIMARY KEY IDENTITY, 
    Name VARCHAR(255) NOT NULL)
);
CREATE TABLE Categories
(
    Id INT PRIMARY KEY IDENTITY, 
    Name VARCHAR(255) NOT NULL)
);
CREATE TABLE ProductsCategories
(
    ProductId INT NOT NULL,
    CategoryId INT NOT NULL,
    FOREIGN KEY (ProductId) REFERENCES Products (Id) [ON DELETE CASCADE],
    FOREIGN KEY (CategoryId) REFERENCES Categories (Id) [ON DELETE CASCADE]
);

```

<p>
  Между продуктами и категориями установлена связь многие-ко-многим, поэтому была создана таблица зависимая ProductsCategories.
</p>
<p>
  Допустим, что после этого база данных была заполнена пользователем необходимыми данными. Тогда запрос для выбора всех пар «Имя продукта – Имя категории» выглядит следующим образом:
</p>

```
    SELECT Products.name AS product, Categories.name AS category FROM Products
    LEFT JOIN ProductsCategories ON Products.Id = ProductsCategories.ProductId
    JOIN Categories ON Categories.Id = ProductsCategories.CategoryId;
```

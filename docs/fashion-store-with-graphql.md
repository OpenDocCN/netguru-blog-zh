# 带 GraphQL 的时装店

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/fashion-store-with-graphql>

在 GraphQL 中，我们有三种类型的操作:

*   获取数据的查询
*   写入数据的突变
*   接收实时数据的订阅

## 时装商店

有一大堆与搜索和订购产品相关的操作用于创建在线商店体验。GraphQL 可以使这个商店更快、更简单地创建和维护。这就是为什么我们想检查我们是否可以一起使用 Rails 和 GraphQL 轻松实现这种类型的商店所需的所有功能。

### 浏览产品

商店的基本功能是浏览产品。我们应该能够浏览所有产品或只显示特定类别的产品。该系统必须获取每个产品的信息，包括它们的参数、可用的变体、颜色和尺寸。GraphQL 可以列出指定的数据，并从相关模型中收集信息，例如关于产品类别的信息。我们可以使用 GraphQL 查询来创建它。我们可以向模式提供关于产品的所有信息。您可以更改前端获取的数据量，而无需对后端进行任何修改。在 Rails 中，我们可以通过声明产品的指定类型来实现。

```
class ProductType < Types::BaseObject
   field :id, ID, null: false
   field :name, String, null: false
   field :price, Integer, null: true
   field :description, String, null: true
   field :product_category, Types::ProductCategoryType, null: true
   field :product_variants, [Types::ProductVariantType], null: true
 end 
```

除了数据字段之外，该类型还包含与其他类型的关系，这些关系可以提供关于产品的附加信息。然后，我们应该将这种类型的字段添加到查询类型中，使其可以从查询中直接访问。我们可以对一个产品和列出所有产品使用相同的类型。

```
 module Types
 class QueryType < Types::BaseObject
   field :products,
         [Types::ProductType],
         null: false,
         description: "Returns a list of products in fashion store"

   field :product,
         Types::ProductType,
         null: false,
         description: "Return product by ID" do
           argument :id, ID, required: true
         end

   def products
     Product.all
   End

   def product(id:)
     Product.find(id)
   end
 end
end 
```

我们为其他模型定义了类似的类型。这种方法使我们能够很容易地确定正在查看的产品所属的类别。

使用 GraphQL，我们不必在显示列表时下载所有产品信息，我们只需在显示所选产品时下载即可。查询是使用特殊的 GQL 语法创建的，它指定了我们想要获取的数据。

关于可用数据的所有信息都包含在自我记录的模式中。前端开发人员可以通过使用 GraphiQL 工具探索关于模式和数据类型的信息，而无需查看后端代码。这有助于创建和测试查询。

搜索产品

![Untitled-2](img/bf47ac4c3c2eb2858c49902082494a22.png)

在我们的商店，我们可以通过名称或描述来搜索产品。因此，有必要在我们的后端实现这一点。在 GraphQL 中，我们可以为每个定义的类型添加过滤器。为此，您需要创建一个单独的过滤器类。

### 在过滤器中，我们定义了参数，这些参数将用于确定如何过滤下载的数据。参数定义用于过滤下载数据的数据类型。然后我们决定如何过滤数据。

确定最低和最高价格的方法完全相同。在本例中，我们添加了指定最高和最低价格的参数。

```
class ProductFilter < ::Types::BaseInputObject
   argument :OR, [self], required: false
   argument :name_contains, String, required: false
   argument :description_contains, String, required: false
   argument :price_min, Integer, required: false
   argument :price_max, Integer, required: false
 end 
```

在大多数情况下，在商店这样的应用程序中，我们不希望立即加载所有产品，因为这会给加载性能带来不必要的压力。分页也可以使用 graphQl 来完成。为此，我们通过添加**:第一个**和**:跳过**选项来修改过滤器类。这些选项允许您限制加载的记录数量。同样的方法有助于显示所选类别中的下一个和上一个产品。

```
def normalize_filters(value, branches = [])
   scope = Product.all
   scope = scope.where('name LIKE ?', "%#{value[:name_contains]}%") if value[:name_contains]
   scope = scope.where('description LIKE ?', "%#{value[:description_contains]}%") if value[:description_contains]
   scope = scope.where('price_cents >= ?', "#{value[:price_min].to_money.cents}") if value[:price_min]
   scope = scope.where('price_cents <= ?', "#{value[:price_max].to_money.cents}") if value[:price_max] 
```

将产品添加到购物车并采购

要对数据库进行任何更改，例如向购物车中添加产品，您必须创建一个负责给定操作的变异。在“突变类型”类中，我们必须添加与我们创建的突变相对应的字段。

```
 option :first, type: types.Int, with: :apply_first
 option :skip, type: types.Int, with: :apply_skip

 def apply_first(scope, value)
   scope.limit(value)
 end

 def apply_skip(scope, value)
   scope.offset(value)
 end
```

### 接下来，我们创建类似控制器的变异类，负责执行单独的动作。在每个这样的类中，我们指定执行特定操作所需的参数。

**解决**是我们的行动方法。我们可以在这里实现执行变异所需的所有逻辑。

```
class MutationType < Types::BaseObject
  field :create_cart_item, mutation: Mutations::CreateCartItem
end 
```

以类似的方式，我们可以执行诸如更改购物车中的产品数量或订购产品之类的操作。

```
class UpdateCartItem < BaseMutation
   argument :product_id, Integer, required: false
   argument :quantity, Integer, required: false
   argument :product_variant_id, Integer, required: false
end 
```

结论

```
def resolve(product_id:, product_variant_id:, quantity:)
  cart_item = Cart.first.cart_items.new(product_id: product_id,
                                           product_variant_id: product_variant_id,
                                           quantity: quantity)
  if cart_item.save
    {
      cart_item: cart_item,
      errors: []
    }
  else
    {
      cart_item: nil,
      errors: cart_item.errors.full_messages
    }
  end
end 
```

GraphQL 适合于像我们的商店这样的应用程序，在那里我们有我们想要查看的产品，这些产品通常引用一个父关系，比如产品类别或当前用户的购物车。我们可以很容易地向前端开发人员提供所有必要的数据。

### 进一步阅读

GraphQL is suitable for applications such as our store, where we have products that we want to view, which mostly reference to one parent relation such as product categories or the current user’s cart. We can provide all the necessary data easily and in a way legible to [frontend developers](/web/20221007201052/https://www.netguru.com/services/front-end-development).

### Further reading
# Sơ đồ cấu trúc Route CRUD
```bash
// routes.jsx
const routes = [
  {
    path: "/products",
    Component: ProductLayout,
    middleware: [authMiddleware],
    children: [
      // 1. READ (Danh sách)
      {
        index: true,
        Component: ProductList,
        loader: async () => fetch("/api/products"),
      },
      // 2. CREATE (Thêm mới)
      {
        path: "create",
        Component: ProductCreateForm,
        // Action xử lý khi nhấn Submit ở form Create
        action: async ({ request }) => {
          const formData = await request.formData();
          const response = await fetch("/api/products", {
            method: "POST",
            body: formData,
          });
          if (response.ok) return redirect("/products");
          return { error: "Failed to create" };
        },
      },
      // 3. UPDATE & DELETE (Dựa trên ID)
      {
        path: ":id",
        // Loader lấy dữ liệu của 1 sản phẩm để điền vào form Edit
        loader: async ({ params }) => fetch(`/api/products/${params.id}`),
        // Component dùng chung cho cả xem chi tiết và sửa
        Component: ProductDetailOrEdit,
        // Action xử lý cả UPDATE và DELETE dựa trên phương thức gửi lên
        action: async ({ request, params }) => {
          const method = request.method; // 'PUT' hoặc 'DELETE'
          
          if (method === "DELETE") {
            await fetch(`/api/products/${params.id}`, { method: "DELETE" });
            return redirect("/products");
          }

          const formData = await request.formData();
          await fetch(`/api/products/${params.id}`, {
            method: "PUT",
            body: formData,
          });
          return redirect("/products");
        }
      }
    ]
  }
];
```
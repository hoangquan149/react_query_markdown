# REACT-QUERY

## QueryClient

-  `QueryClient` sử dụng để tương tác với bộ nhớ cache:

```
import { QueryClient } from '@tanstack/react-query'

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: Infinity,
    },
  },
})

await queryClient.prefetchQuery(['posts'], fetchPosts)
```

#### `queryClient.setQueryData`

-  `setQueryData` là một hàm có thể cập nhập trức tiếp data đã lưu trong bộ nhớ cache query.

VD:

```
const useUpdateTitle = (id) => {
  const queryClient = useQueryClient()

  return useMutation(
    (newTitle) => axios.patch(`/posts/${id}`, { title: newTitle }),
    {
      onSuccess: (newPost) => {
        queryClient.setQueryData(['posts', id], newPost)
      },
    }
  )
}
```

#### `queryClient.setQueriesData`

-  `setQueriesData` là một chức năng đồng bộ sử dụn để cập nhật ngay lập tưc data đã lưu trong cache của nhiều query

## useQuery

-  `useQuery` hook giúp chúng ta fetch data và kiểm soát state của data được truy suất

VD:

```
import { useQuery } from 'react-query'

const fetchMovies = async () => {
  const response = await fetch('my url')
  return response.json()
}

function App() {
    const { isLoading, isError, data, error } = useQuery('key-unique', fetchMovies)
}
```

-  Ví dụ trên , `fetchMovies` là một đoạn code không đồng bộ trả về một mảng có tất cả các `movies`.

   -  `data` hook sẽ trả dữ liệu về cho mình qua biến data
   -  `isLoading` trả về trạng thái loading (true) khi data đang load
   -  `isError` trả về lỗi khi fetch data bị lỗi
   -  `error` sẽ trả về lỗi
   -  `key-unique` mỗi query sẽ có một key duy nhất và tên không được trùng nhau

-  Trường hợp mà cần phải truyền cảparams

VD:

```
const useFetchMovie = (id) => {
   return useQuery(["movie", id], () =>
      fetch(`my-url/${id}`).then((res) =>
         res.json()
      )
   );
};
```

## useMutation

-  Không giống như các query , `useMutation` thường được sử dụng để `create/update/delele` dữ liệu

VD:

```
function App() {
  const { isLoading, status, isError } = useMutation(newTodo => {
    return axios.post('/todos', newTodo)
  })

  return (
    <div>
        <button
            onClick={() => {
                mutation.mutate({ id: new Date(), title: 'Do Laundry' })
            }}
        >
        Create Todo
        </button>
    </div>
  )
}
```

-  `useMutation` cũng có những trạng thái như:
   -  `isIdle` hoặc `status === 'idle'`mutation không hoạt động hoặc fresh/reset state
   -  `isLoading` hoặc `status === 'loading'` khi mutation đang chạy
   -  `isError` hoặc `status === 'error'` khi mutation gặp lỗi
   -  `isSuccess` hoặc `status === 'success'` mutation thành công khi data mutation có sẵn

---

### Ưu điểm của của `react-query`

-  Catching data cho API
-  Hạn chế gọi nhiều request trùng nhau
-  Tự động cập nhật data của API, giúp data luôn mới và đồng bộ với server
-  Phân trang và lazy loading
-  Điều khiển được data khi nó bị cũ, có thể gọi lại dễ dàng
-  Giúp tăng trải nghiệm UX cho web app với “instant” data

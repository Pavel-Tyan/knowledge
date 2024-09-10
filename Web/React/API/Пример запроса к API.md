Несколько важных моментов:\
1. Обработка ошибок
2. Отображение статуса загрузки
3. Отмена предыдущего запроса для борьбы с race condition (В данном случае используется AbortController)

```tsx
import { useEffect, useRef, useState } from "react";

const BASE_URL = "<https://jsonplaceholder.typicode.com>";

interface Post {
  id: number;
  title: string;
}

export default function Demo() {
  const [error, setError] = useState();
  const [isLoading, setIsLoading] = useState(false);
  const [posts, setPosts] = useState<Post[]>([]);
  const [page, setPage] = useState(0);

  const abortControllerRef = useRef<AbortController | null>(null);

  useEffect(() => {
    const fetchPosts = async () => {
      abortControllerRef.current?.abort();
      abortControllerRef.current = new AbortController();

      setIsLoading(true);

      try {
        const response = await fetch(`${BASE_URL}/posts?page=${page}`, {
          signal: abortControllerRef.current?.signal,
        });
        const posts = (await response.json()) as Post[];
        setPosts(posts);
      } catch (e: any) {
        if (e.name === "AbortError") {
          console.log("Aborted");
          return;
        }

        setError(e);
      } finally {
        setIsLoading(false);
      }
    };

    fetchPosts();
  }, [page]);

  if (error) {
    return <div>Something went wrong! Please try again.</div>;
  }

  return (
    <div className="tutorial">
      <h1 className="mb-4 text-2xl">Data Fething in React</h1>
      <button onClick={() => setPage(page + 1)}>Increase Page ({page})</button>
      {isLoading && <div>Loading...</div>}
      {!isLoading && (
        <ul>
          {posts.map((post) => {
            return <li key={post.id}>{post.title}</li>;
          })}
        </ul>
      )}
    </div>
  );
}
```

Также лучше отменить запрос при unmountе страницы

```tsx
useEffect(() => {
	return () => abortController.current?.abort?.();
}, []);
```
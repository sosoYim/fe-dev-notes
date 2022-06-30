## `queryClient.fetchQuery`[](https://react-query.tanstack.com/reference/QueryClient#queryclientfetchquery "Direct link to heading")

`fetchQuery` is an asynchronous method that can be used to fetch and cache a query. It will either resolve with the data or throw with the error. Use the `prefetchQuery` method if you just want to fetch a query without needing the result.

If the query exists and the data is not invalidated or older than the given `staleTime`, then the data from the cache will be returned. Otherwise it will try to fetch the latest data.

캐시에 남아있고 최신 정보일 때 굳이 범위를 접하시면 될듯

**Options**

The options for `fetchInfiniteQuery` are exactly the same as those of [`fetchQuery`](https://react-query.tanstack.com/reference/QueryClient#queryclientfetchquery).

Since only successful queries are included per default, to also include `Errors`, you have to provide `shouldDehydrateQuery`,
``


**Returns**

-   `Promise<InfiniteData<TData>>`
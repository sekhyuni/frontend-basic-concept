# React Query

* [Life-Cycle](#life-cycle)
* [useQuery](#usequery)
* [useMutation](#usemutation)
* [QueryClient](#queryclient)

## Life-Cycle
1. 컴포넌트 Mount 시, useQuery 인스턴스 Mount
1. 데이터 fetch 후, query key별로 데이터 캐싱
1. staleTime이 지난 뒤, 해당 데이터는 fresh 상태에서 stale 상태로 변경됨
1. 컴포넌트 Unmount 시, useQuery 인스턴스 Unmount
1. useQuery 인스턴스가 Unmount된 이후 cacheTime이 지나기 전에 다시 useQuery 인스턴스가 Mount되면 캐싱된 데이터가 적용되며, 이와는 별개로 네트워크 요청은 보낸 뒤, 응답이 오면 fresh한 데이터로 교체됨

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#react-query)
## useQuery
1. Options
    1. staleTime
        - 기본값: 0ms
        - 데이터 fetch 후, staleTime 동안 네트워크 요청 자체를 보내지 않음
        - Cache-Control 속성의 max-age값과 동일한 맥락으로 동작
    1. cacheTime
        - 기본값: 300,000ms(5min)
        - useQuery 인스턴스가 Unmount된 이후 cacheTime이 지나면 캐싱된 데이터가 제거됨
        - useQuery 인스턴스가 Unmount된 이후 cacheTime이 지나기 전에 다시 useQuery 인스턴스가 Mount되면 캐싱된 데이터가 적용되며, 이와는 별개로 네트워크 요청은 보낸 뒤, 응답이 오면 fresh한 데이터로 교체됨

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#react-query)
## useMutation
1. Options
- Update later..

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#react-query)
## QueryClient
1. queryClient.refetchQueries
    - 쿼리가 마운트되지 않은 상태에도 queryKey와 일치하는 모든 새 쿼리를 즉시 요청
1. queryClient.invalidateQueries
    - 쿼리가 마운트되지 않은 상태인 경우 queryKey와 일치하는 모든 새 쿼리를 즉시 요청하지 않으며, 쿼리가 마운트된 경우 staleTime 설정 유무와 관계없이 캐싱된 데이터를 무효화하고 새 쿼리를 요청
    - 쿼리가 마운트되지 않은 상태에서 mutate를 하는 경우, refetchQueries보다 invalidateQueries를 사용함으로써 네트워크 요청 횟수를 줄일 수 있음 (일반적인 상황에서는 invalidateQueries를 사용하는 것이 좋음)

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#react-query)
# Architecture

* [Clean Architecture](#clean-architecture)

## Clean Architecture  
![Clean Architecture](./assets/img/clean-architecture.png)
- Layers
    - Frameworks and Drivers
        - 정의: MongoDB Node.js Driver, firestore와 같은 Driver 또는 mongoose와 같은 ORM을 사용해서 DB Connection을 맺고 CRUD를 하는 등의 작업을 수행
            ```typescript
            // src/frameworks/getArticleListAPI.ts
            import { firestore } from './admin';

            interface GetArticleResponse {
                id: string;
                media: string;
                title: string;
                createdAt: { _seconds: number };
                link: string;
            }

            export async function getArticleListAPI(): Promise<GetArticleResponse[]> {
                const articlesCollectionRef = firestore.collection('articles');
                const articlesQuerySnapshot = await articlesCollectionRef.orderBy('createdAt', 'desc').get();

                const response = articlesQuerySnapshot.docs.map((articleQuerySnapshot) => ({
                    ...articleQuerySnapshot.data(),
                    id: articleQuerySnapshot.id,
                } as GetArticleResponse));

                return response;
            }
            ```
    - Interface Adapters (Infrastructure)
        - 정의: 외부 요소(DB 또는 Web)로부터 얻은 데이터를 Entities 또는 Use Cases에 적합한 형식으로 변환하거나, 그 반대의 작업을 수행  
            ```typescript
            // src/infrastructures/repository/articleAPIRepository.ts
            import type { Article, ArticleRepository } from '@/entities/article';
            import { getArticleListAPI } from '@/frameworks/getArticleListAPI';

            export default class ArticleAPIRepository implements ArticleRepository {
                async getArticleList(): Promise<Article[]> {
                    const articleList = await getArticleListAPI();

                    return articleList.map((article) => {
                        const createdAtTimestamp = article.createdAt._seconds * 1000;

                        return {
                            ...article,
                            createdAt: createdAtTimestamp
                        }
                    });
                }
            }
            ```
    - Use Cases
        - 정의: 애플리케이션의 특정한 사용 사례나 시나리오를 구현하며, Entities에는 의존하지만 외부 요소들에는 의존하지 않음
            ```typescript
            // src/usecases/articleService.ts
            import type { Article, ArticleRepository } from '@/entities/article';

            export class ArticleService {
                private readonly repository: ArticleRepository | null;

                constructor(repository: ArticleRepository) {
                    this.repository = repository ?? null;
                }

                getArticleList(): Promise<Article[]> {
                    if (!this.repository) {
                        throw new Error('repository is required');
                    }

                    const articleList = this.repository.getArticleList();

                    return articleList;
                }
            }
            ```
    - Entities
        - 정의: 애플리케이션의 비즈니스 로직을 포함하며, 어떠한 외부 요소에도 의존하지 않는 순수한 비즈니스 규칙을 담음
            ```typescript
            // src/entities/article.ts
            export interface Article {
                id: string;
                media: string;
                title: string;
                createdAt: number;
                link: string;
            }

            export interface ArticleRepository {
                getArticleList(): Promise<Article[]>;
            }
            ```
- 사용 예시 (Next.js 13 App Router)
    ```tsx
    import { ArticleService } from '@/usecases/articleService';
    import ArticleAPIRepository from '@/infrastructures/repository/articleAPIRepository';

    import ArticleSection from '@/components/ArticleSection';

    export default async function Home() {
        const articleService = new ArticleService(new ArticleAPIRepository());

        const articleList = await articleService.getArticleList();

        return (
            <>
                <ArticleSection articleList={articleList} />
            </>
        );
    }
    ```

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#architecture)
## 협업 필터링 알고리즘 (Collaborative Filtering)

- K-nearest neighbor (KNN) 알고리즘
- Matrix factorization 알고리즘
- Matrix factorization + PLSI 알고리즘

- sparse matrix (희소 행렬) 형태를 사용해야 프로젝트를 수행할 수 있다!
    - ⇒ dense matrix 형태로 만들면 너무 느림

## 개발 환경 실행 (movieRecProject 기준)

1. 프로젝트로 이동
2. anaconda prompt에서 다음 명령어 실행

```bash
python manage.py migrate
python manage.py runserver # 웹 서버 구동
```

1. `[http://127.0.0.1:8000/](http://127.0.0.1:8000/)` 을 통해 확인

## run_kMM()

- train.py의 입력 파라미터
    - -i : 데이터 파일 디렉토리
    - -o : 결과 파일 디렉토리
    - -a : 0 (knn 학습 알고리즘을 나타냄)
- load_result() 함수를 ‘matrixfactorization’를 입력으로 호출

## load_result()

- SQLite3 데이터베이스에 ㅇ녀결
- 예상 영화 평점 데이터를 load_movies, load_viewed, load_recomm 함수들을 각각 호출해서 SQLite3 데이터베이스에 있는 테이블들에 저장

## [loadResult.py](http://loadResult.py) - load_movies()

- `ml-1m_movies.dat` 데이터 파일에서 모든 영화의 id, title, text를 읽음
    - 튜플 (id, title, text)를 만들어서 `movieRec_movie` 테이블에 삽입
- 데이터 저장 형태 ⇒ `movie-id::movie-title(year)::genre1|genre2|...|genreN`
    - (ex) `1::Toy Story (1995)::Animation|Comedy`
    

## [loadResult.py](http://loadResult.py) - load_viewed()

- SQLite3 데이터베이스에 있는 `movieRec_viewed` 테이블의 내용을 삭제
- 고객이 본 영화들의 평점이 들어있는 `train_ratings.txt` 파일을 읽음
    - `user_id`, `movie_id`, `rating`을 SQLite3의 `movieRec_viewed`에 삽입
- `user_id`가 `movieRec_user` 테이블에 저장되어 있지 않으면?
    - `movieRec_user` 테이블에 고객 id와 이름을 삽입
    - 고객 id : `user_id` 사용
    - 고객 이름 : `User`와 `user_id`를 concatenation한 스트링
    

## ✨load_recomm() 함수 ⇒ 우리가 구현해야 함!!

- 추천 알고리즘에서 생성한 추천 결과를 읽어 들이는데 사용한다.
- 맨 먼저 SQLite3의 `movieRec_recomm` 테이블의 내용을 삭제
- 예상 평점이 들어 있는 `recommend_ratings.txt` 파일은 `user_id::movie_id::score` 형태로 저장되어 있음
    - 모든 데이터를 읽어서 `user_id`, `movie_id`, `score`를 SQLite3의 `movieRec_recomm` 테이블에 삽입한다.
├── application/
│   ├── dto
│   └── controller
│
├── domain/
│   ├── common
│       └── enums/
│       └── dto/
│   ├── customer/
│   │   └── entity/
│   │   └── service/
│   │   └── repository/
│   ├── movie/
│   │   └── entity/
│   │   └── service/
│   │   └── repository/
│   ├── reservation
│   │   └── entity/
│   │   └── service/
│   │   └── repository/
│
└── infra/
└── ... redis 추가를 위한 모듈

- application
  - 외부 통신 담당
- domain
  - 도메인 관련 로직 담당
- infra
  - redis 및 외부 라이브러리 모듈 담당

------------------------------------------------------------------------------------------------------------------------

DB 설계
CREATE TABLE `customers` (
`id` bigint NOT NULL AUTO_INCREMENT,
`created_at` timestamp NULL DEFAULT NULL,
`created_by` varchar(255) DEFAULT NULL,
`updated_at` timestamp NULL DEFAULT NULL,
`updated_by` varchar(255) DEFAULT NULL,
`email` varchar(100) DEFAULT NOT NULL,
`password` varchar(100) DEFAULT NOT NULL
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

CREATE TABLE `movie` (
`id` bigint NOT NULL AUTO_INCREMENT,
`created_at` timestamp NULL DEFAULT NULL,
`created_by` varchar(255) DEFAULT NULL,
`updated_at` timestamp NULL DEFAULT NULL,
`updated_by` varchar(255) DEFAULT NULL,
`title` varchar(100) NOT NULL COMMENT '영화 제목',
`thumbnail` varchar(500) NOT NULL COMMENT '영화 포스터 URL',
`movie_ratings` int NOT NULL COMMENT '영상물 등급',
`release_date` date NOT NULL COMMENT '영화 개봉일',
`running_time` int NOT NULL COMMENT '영화 러닝타임',
`theater_name` varchar(100) NOT NULL COMMENT '상영관 이름',
`genre` varchar(100) NOT NULL COMMENT '영화 장르'
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

CREATE TABLE `showing_movie` (
`id` bigint NOT NULL AUTO_INCREMENT,
`created_at` timestamp NULL DEFAULT NULL,
`created_by` varchar(255) DEFAULT NULL,
`updated_at` timestamp NULL DEFAULT NULL,
`updated_by` varchar(255) DEFAULT NULL,
`end_date` date NOT NULL COMMENT '영화 종영일',
`movie_id` bigint DEFAULT NULL,
KEY `FK_movie_id_of_showing_movie` (`movie_id`),
CONSTRAINT `FK_movie_id_of_showing_movie` FOREIGN KEY (`movie_id`) REFERENCES `movie` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

CREATE TABLE `reservation` (
`id` bigint NOT NULL AUTO_INCREMENT,
`created_at` timestamp NULL DEFAULT NULL,
`created_by` varchar(255) DEFAULT NULL,
`updated_at` timestamp NULL DEFAULT NULL,
`updated_by` varchar(255) DEFAULT NULL,
`reservation_date` date NOT NULL COMMENT '영화관람일',
`reservation_time` time NOT NULL COMMENT '영화 시작 시간',
`seat` varchar(50) NOT NULL COMMENT '좌석 정보',
`customers_id` bigint DEFAULT NULL,
`movie_id` bigint DEFAULT NULL,
KEY `FK_customers_id_of_reservation` (`customers_id`),
KEY `FK_movie_id_of_reservation` (`movie_id`),
CONSTRAINT `FK_customers_id_of_reservation` FOREIGN KEY (`customers_id`) REFERENCES `customers` (`id`),
CONSTRAINT `FK_movie_id_of_reservation` FOREIGN KEY (`movie_id`) REFERENCES `movie` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

CREATE TABLE `show_schedule` (
`id` bigint NOT NULL AUTO_INCREMENT,
`created_at` timestamp NULL DEFAULT NULL,
`created_by` varchar(255) DEFAULT NULL,
`updated_at` timestamp NULL DEFAULT NULL,
`updated_by` varchar(255) DEFAULT NULL,
`start_show_time` time NOT NULL COMMENT '영화 시작 시간'
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

CREATE TABLE `movie_show_schedule` (
`id` bigint NOT NULL AUTO_INCREMENT,
`created_at` timestamp NULL DEFAULT NULL,
`created_by` varchar(255) DEFAULT NULL,
`updated_at` timestamp NULL DEFAULT NULL,
`updated_by` varchar(255) DEFAULT NULL,
`movie_id` bigint DEFAULT NULL,
`show_schedule_id` bigint DEFAULT NULL,

KEY `FK_movie_id_of_movie_show_schedule` (`movie_id`),
KEY `FK_show_schedule_id_of_movie_show_schedule` (`show_schedule_id`),
CONSTRAINT `FK_movie_id_of_movie_show_schedule` FOREIGN KEY (`movie_id`) REFERENCES `movie` (`id`),
CONSTRAINT `FK_show_schedule_id_of_reservation` FOREIGN KEY (`show_schedule_id`) REFERENCES `show_schedule` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

------------------------------------------------------------------------------------------------------------------------
아직 1주차 상영 중인 영화 조회 API 과제를 완료하지 못했고, 기본적인 테이블 설계와 모듈 설계만 끝냈습니다.

1주차 과제를 진행하면서 어려웠던 것은 멀티 모듈을 직접 설계하여 사용하는건 이번이 처음이기 때문에 각 모듈의 기능이 어디까지
독립적으로 이루어지는거 효율적인지 파악이 힘들었고, 모듈을 만들고 각 모듈을 import 해서 사용하는 방법이 어렵습니다. 
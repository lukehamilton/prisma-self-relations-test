# Migration `20200215193722-init`

This migration has been generated by Luke Hamilton at 2/15/2020, 7:37:22 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
CREATE TABLE "public"."User" (
    "address" text  NOT NULL DEFAULT '',
    "auth0id" text  NOT NULL DEFAULT '',
    "avatar" text   ,
    "createdAt" timestamp(3)  NOT NULL DEFAULT '1970-01-01 00:00:00',
    "email" text  NOT NULL DEFAULT '',
    "firstName" text   ,
    "headline" text   ,
    "id" text  NOT NULL ,
    "identity" text   ,
    "lastName" text   ,
    "name" text  NOT NULL DEFAULT '',
    "name_lower" text  NOT NULL DEFAULT '',
    "privateKey" text  NOT NULL DEFAULT '',
    "role" text  NOT NULL DEFAULT 'USER',
    "updatedAt" timestamp(3)  NOT NULL DEFAULT '1970-01-01 00:00:00',
    "username" text  NOT NULL DEFAULT '',
    "username_lower" text  NOT NULL DEFAULT '',
    PRIMARY KEY ("id")
) 

CREATE TABLE "public"."Topic" (
    "createdAt" timestamp(3)  NOT NULL DEFAULT '1970-01-01 00:00:00',
    "description" text   ,
    "followersCount" integer   ,
    "id" text  NOT NULL ,
    "image" text   ,
    "name" text  NOT NULL DEFAULT '',
    "postsCount" integer   ,
    "slug" text  NOT NULL DEFAULT '',
    "trending" boolean   ,
    "updatedAt" timestamp(3)  NOT NULL DEFAULT '1970-01-01 00:00:00',
    PRIMARY KEY ("id")
) 

CREATE TABLE "public"."Post" (
    "createdAt" timestamp(3)  NOT NULL DEFAULT '1970-01-01 00:00:00',
    "day" text  NOT NULL DEFAULT '',
    "description" text   ,
    "featured" boolean   ,
    "id" text  NOT NULL ,
    "link" text   ,
    "name" text   ,
    "slug" text  NOT NULL DEFAULT '',
    "submitter" text  NOT NULL ,
    "tagline" text   ,
    "thumbnail" text   ,
    "type" text  NOT NULL DEFAULT 'PRODUCT',
    "updatedAt" timestamp(3)  NOT NULL DEFAULT '1970-01-01 00:00:00',
    PRIMARY KEY ("id")
) 

CREATE TABLE "public"."Comment" (
    "author" text  NOT NULL ,
    "createdAt" timestamp(3)  NOT NULL DEFAULT '1970-01-01 00:00:00',
    "id" text  NOT NULL ,
    "parent" text   ,
    "post" text   ,
    "text" text  NOT NULL DEFAULT '',
    "updatedAt" timestamp(3)  NOT NULL DEFAULT '1970-01-01 00:00:00',
    PRIMARY KEY ("id")
) 

CREATE TABLE "public"."CommentVote" (
    "comment" text  NOT NULL ,
    "createdAt" timestamp(3)  NOT NULL DEFAULT '1970-01-01 00:00:00',
    "id" text  NOT NULL ,
    "updatedAt" timestamp(3)  NOT NULL DEFAULT '1970-01-01 00:00:00',
    "user" text  NOT NULL ,
    PRIMARY KEY ("id")
) 

CREATE TABLE "public"."Vote" (
    "createdAt" timestamp(3)  NOT NULL DEFAULT '1970-01-01 00:00:00',
    "id" text  NOT NULL ,
    "post" text  NOT NULL ,
    "updatedAt" timestamp(3)  NOT NULL DEFAULT '1970-01-01 00:00:00',
    "user" text  NOT NULL ,
    PRIMARY KEY ("id")
) 

CREATE TABLE "public"."Section" (
    "createdAt" timestamp(3)  NOT NULL DEFAULT '1970-01-01 00:00:00',
    "date" text  NOT NULL DEFAULT '',
    "id" text  NOT NULL ,
    "updatedAt" timestamp(3)  NOT NULL DEFAULT '1970-01-01 00:00:00',
    PRIMARY KEY ("id")
) 

CREATE TABLE "public"."SignedUpload" (
    "createdAt" timestamp(3)  NOT NULL DEFAULT '1970-01-01 00:00:00',
    "id" text  NOT NULL ,
    "signedRequest" text  NOT NULL DEFAULT '',
    "updatedAt" timestamp(3)  NOT NULL DEFAULT '1970-01-01 00:00:00',
    "url" text  NOT NULL DEFAULT '',
    PRIMARY KEY ("id")
) 

CREATE TABLE "public"."_follows" (
    "A" text  NOT NULL ,
    "B" text  NOT NULL 
) 

CREATE TABLE "public"."_CreatedPosts" (
    "A" text  NOT NULL ,
    "B" text  NOT NULL 
) 

CREATE TABLE "public"."_TopicToUser" (
    "A" text  NOT NULL ,
    "B" text  NOT NULL 
) 

CREATE TABLE "public"."_PostToTopic" (
    "A" text  NOT NULL ,
    "B" text  NOT NULL 
) 

CREATE TABLE "public"."_PostToSection" (
    "A" text  NOT NULL ,
    "B" text  NOT NULL 
) 

CREATE UNIQUE INDEX "User.email" ON "public"."User"("email")

CREATE UNIQUE INDEX "User.username" ON "public"."User"("username")

CREATE UNIQUE INDEX "User.username_lower" ON "public"."User"("username_lower")

CREATE UNIQUE INDEX "User.auth0id" ON "public"."User"("auth0id")

CREATE UNIQUE INDEX "Topic.slug" ON "public"."Topic"("slug")

CREATE UNIQUE INDEX "Post.slug" ON "public"."Post"("slug")

CREATE UNIQUE INDEX "Section.date" ON "public"."Section"("date")

CREATE UNIQUE INDEX "_follows_AB_unique" ON "public"."_follows"("A","B")

CREATE UNIQUE INDEX "_CreatedPosts_AB_unique" ON "public"."_CreatedPosts"("A","B")

CREATE UNIQUE INDEX "_TopicToUser_AB_unique" ON "public"."_TopicToUser"("A","B")

CREATE UNIQUE INDEX "_PostToTopic_AB_unique" ON "public"."_PostToTopic"("A","B")

CREATE UNIQUE INDEX "_PostToSection_AB_unique" ON "public"."_PostToSection"("A","B")

ALTER TABLE "public"."Post" ADD FOREIGN KEY ("submitter") REFERENCES "public"."User"("id") ON DELETE RESTRICT

ALTER TABLE "public"."Comment" ADD FOREIGN KEY ("author") REFERENCES "public"."User"("id") ON DELETE RESTRICT

ALTER TABLE "public"."Comment" ADD FOREIGN KEY ("post") REFERENCES "public"."Post"("id") ON DELETE SET NULL

ALTER TABLE "public"."Comment" ADD FOREIGN KEY ("parent") REFERENCES "public"."Comment"("id") ON DELETE SET NULL

ALTER TABLE "public"."CommentVote" ADD FOREIGN KEY ("user") REFERENCES "public"."User"("id") ON DELETE RESTRICT

ALTER TABLE "public"."CommentVote" ADD FOREIGN KEY ("comment") REFERENCES "public"."Comment"("id") ON DELETE RESTRICT

ALTER TABLE "public"."Vote" ADD FOREIGN KEY ("user") REFERENCES "public"."User"("id") ON DELETE RESTRICT

ALTER TABLE "public"."Vote" ADD FOREIGN KEY ("post") REFERENCES "public"."Post"("id") ON DELETE RESTRICT

ALTER TABLE "public"."_follows" ADD FOREIGN KEY ("A") REFERENCES "public"."User"("id") ON DELETE CASCADE

ALTER TABLE "public"."_follows" ADD FOREIGN KEY ("B") REFERENCES "public"."User"("id") ON DELETE CASCADE

ALTER TABLE "public"."_CreatedPosts" ADD FOREIGN KEY ("A") REFERENCES "public"."Post"("id") ON DELETE CASCADE

ALTER TABLE "public"."_CreatedPosts" ADD FOREIGN KEY ("B") REFERENCES "public"."User"("id") ON DELETE CASCADE

ALTER TABLE "public"."_TopicToUser" ADD FOREIGN KEY ("A") REFERENCES "public"."Topic"("id") ON DELETE CASCADE

ALTER TABLE "public"."_TopicToUser" ADD FOREIGN KEY ("B") REFERENCES "public"."User"("id") ON DELETE CASCADE

ALTER TABLE "public"."_PostToTopic" ADD FOREIGN KEY ("A") REFERENCES "public"."Post"("id") ON DELETE CASCADE

ALTER TABLE "public"."_PostToTopic" ADD FOREIGN KEY ("B") REFERENCES "public"."Topic"("id") ON DELETE CASCADE

ALTER TABLE "public"."_PostToSection" ADD FOREIGN KEY ("A") REFERENCES "public"."Post"("id") ON DELETE CASCADE

ALTER TABLE "public"."_PostToSection" ADD FOREIGN KEY ("B") REFERENCES "public"."Section"("id") ON DELETE CASCADE
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration ..20200215193722-init
--- datamodel.dml
+++ datamodel.dml
@@ -1,0 +1,124 @@
+
+datasource db {
+  provider = "postgresql"
+  url      = "postgresql://test:test@0.0.0.0:5432/test"
+}
+
+generator client {
+  provider = "prisma-client-js"
+}
+
+model User {
+  id               String   @default(cuid()) @id
+  createdAt        DateTime @default(now())
+  updatedAt        DateTime @updatedAt
+  email            String   @unique
+  role             Role     @default(USER)
+  firstName        String?
+  lastName         String?
+  name             String
+  name_lower       String
+  username         String   @unique
+  username_lower   String   @unique
+  headline         String?
+  avatar           String?
+  auth0id          String   @unique
+  identity         String?
+  privateKey       String
+  follows          User[]  @relation(name: "follows")
+  followedBy       User[]  @relation(name: "follows")
+  address          String
+  submittedPosts   Post[]   @relation(name: "SubmittedPosts")
+  createdPosts     Post[]   @relation(name: "CreatedPosts")
+  followedTopics   Topic[]
+}
+
+model Topic {
+  id             String   @default(cuid()) @id
+  createdAt      DateTime @default(now())
+  updatedAt      DateTime @updatedAt
+  name           String
+  slug           String   @unique
+  description    String?
+  image          String?
+  followersCount Int?
+  postsCount     Int?
+  trending       Boolean?
+  posts          Post[]
+  followedBy     User[]
+}
+
+model Post {
+  id            String    @default(cuid()) @id
+  createdAt     DateTime  @default(now())
+  updatedAt     DateTime  @updatedAt
+  type          PostType  @default(PRODUCT)
+  name          String?
+  slug          String    @unique
+  link          String?   
+  thumbnail     String?   
+  description   String?   
+  tagline       String?   
+  submitter     User      @relation(name: "SubmittedPosts")
+  creators      User[]    @relation(name: "CreatedPosts")
+  comments      Comment[] 
+  day           String
+  featured      Boolean?
+  topics        Topic[]
+  votes         Vote[]
+  sections      Section[]
+}
+
+model Comment {
+  id            String         @default(cuid()) @id
+  createdAt     DateTime       @default(now())
+  updatedAt     DateTime       @updatedAt
+  text          String             
+  author        User
+  post          Post?
+  parent        Comment?       @relation(name: "commentReplies")
+  replies       Comment[]      @relation(name: "commentReplies")
+  votes         CommentVote[]
+}
+
+model CommentVote {
+  id            String   @default(cuid()) @id
+  createdAt     DateTime @default(now())
+  updatedAt     DateTime @updatedAt
+  user          User
+  comment       Comment
+}
+
+model Vote {
+  id            String   @default(cuid()) @id
+  createdAt     DateTime @default(now())
+  updatedAt     DateTime @updatedAt
+  user          User
+  post          Post
+}
+
+model Section {
+  id            String   @default(cuid()) @id
+  createdAt     DateTime @default(now())
+  updatedAt     DateTime @updatedAt
+  date          String   @unique
+  posts         Post[]
+}
+
+model SignedUpload {
+  id            String   @default(cuid()) @id
+  createdAt     DateTime @default(now())
+  updatedAt     DateTime @updatedAt
+  signedRequest String
+  url           String
+}
+
+enum Role {
+  ADMIN
+  USER
+}
+
+enum PostType {
+  PRODUCT
+  LINK
+}
```



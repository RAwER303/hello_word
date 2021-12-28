npm install -D prisma-dbml-generator
generator dbml {
  provider = "prisma-dbml-generator"
}
model User {
  id        Int      @id @default(autoincrement())
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  email     String   @unique
  name      String?
  posts     Post[]
  profile   Profile?
  /// user role
  role      Role     @default(USER)
}

/// User profile
model Profile {
  id     Int     @default(autoincrement()) @id
  bio    String?
  user   User    @relation(fields: [userId], references: [id])
  userId Int     @unique
}

model Post {
  id         Int        @id @default(autoincrement())
  title      String     @default("")
  content    String?
  published  Boolean    @default(false)
  author     User?      @relation(fields: [authorId], references: [id])
  authorId   Int?
  categories Category[]
}

model Category {
  id    Int    @id @default(autoincrement())
  name  String
  posts Post[]
}

/// user role
enum Role {
  ADMIN /// allowed to do everything
  USER
}

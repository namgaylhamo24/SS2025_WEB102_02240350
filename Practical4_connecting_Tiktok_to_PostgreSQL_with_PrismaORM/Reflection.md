**Note:** This folder only contains the `README.md` and `Reflection.md` files. The actual implementation of the practical work can be found in the `Practical2_API_Design_and_Implementation_Tiktok` folder.

## Looking Back on My Development Journey

When I first started this project to connect a TikTok-inspired application to PostgreSQL using Prisma ORM, I'll admit I felt both excited and a bit overwhelmed. What began as a simple database connection evolved into a comprehensive learning experience that taught me so much about modern backend development.

## The Real Lessons Learned

### Database Connections Aren't as Scary as They Seem
I remember staring at my first PostgreSQL connection error, completely baffled about why my credentials weren't working. Through trial and error (and plenty of documentation reading), I discovered the importance of:
- Properly formatting connection strings (those special characters matter!)
- Setting up database users with the exact right privileges
- Using environment variables to keep sensitive information secure

### Prisma Became My New Best Friend
At first, writing the Prisma schema felt abstract, but once I saw how it automatically generated types and database clients, I was hooked. Some key moments:
- The "aha!" when I first used Prisma's type-safe queries and my IDE could autocomplete model fields
- The satisfaction of writing a complex query with multiple relations that just worked
- The relief when migrations handled schema changes smoothly (after I learned to use them properly)

### Security is More Than Just an Afterthought
Implementing authentication taught me some hard but valuable lessons:
- My first version stored passwords in plain text (yikes!) until I learned about bcrypt hashing
- JWT tokens seemed magical until I had to debug token expiration issues
- Protecting routes properly required me to really understand middleware

## Overcoming Challenges

### The Great Migration Debacle
I'll never forget the afternoon I spent staring at migration conflicts. I had made changes directly to the database and then tried to generate a migration. The solution? Learning to:
1. Reset the database completely
2. Re-run migrations from scratch
3. Make all schema changes through Prisma first

### When Tests Save the Day
Creating test data revealed flaws in my assumptions about relationships. I kept hitting foreign key errors until I:
- Implemented a clear seeding order (users first, then posts, then comments)
- Used transactions to ensure all or nothing creation
- Added proper error handling for constraint violations

## Personal Takeaways

This project transformed my understanding of backend development from academic to practical. Some key mindset shifts:

1. **Database design requires forethought** - I can't just add columns willy nilly
2. **ORMs are powerful but have learning curves** - Prisma saves time but requires understanding its patterns
3. **Security deserves front-and-center attention** - It's easier to build it in from the start than bolt it on later

## Moving Forward

While I'm proud of what I've accomplished, I'm even more excited about where to improve:
- I want to implement proper backup procedures (better safe than sorry)
- I need to add more robust input validation
- I'm curious to explore database performance monitoring

This project started as a technical exercise but became a real confidence builder. The next time I need to stand up a backend system, I'll approach it with both more humility about the complexities and more confidence in my ability to work through them.

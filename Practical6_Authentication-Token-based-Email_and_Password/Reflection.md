# My Authentication Journey: Wins, Fails, and Lessons
Here's my honest take on building this authentication system - the good, the bad, and the "why did I think that would work?" moments.

## What I Built

I created a secure backend that:

- Lets users register with email/password (passwords safely hashed, of course)
- Issues JWTs that actually expire (unlike my first attempt)
- Protects sensitive data like bank balances
- Handles errors gracefully without leaking system info

## The Reality Check

### Lesson 1: Security is Layers

I started thinking authentication was just checking passwords. Boy was I wrong! Here's what it really involves:

1. **Password Handling**:
   - Learned why we hash (my early "just store it plaintext" idea was terrifying in hindsight)
   - Discovered bcrypt's cost factor is a balancing act between security and performance

2. **Tokens**:
   - My first tokens were basically "hello world" strings with no expiration
   - Learned the hard way about proper payload structure and signature verification

3. **Error Messages**:
   - First version either said "everything is broken" (bad) or "nothing is wrong" when it wasn't (worse)

### The Struggle Was Real

**Oh No, My Tokens!**  
Spent a long hours getting "Invalid Token" errors. Turns out I was sending "Token [value]" instead of "Bearer [value]". Facepalm moment.

**Email Race Conditions**   
Two users registering with same email at once created duplicate accounts. Fixed by adding database constraints AND application checks.

**Testing Headaches**  
My Postman setup looked like spaghetti before I organized it properly with environments and test scripts.

### How I Grew

This project changed me. I now:

- Actually care about proper error handling
- Appreciate middleware magic
- Understand why security can't be an afterthought



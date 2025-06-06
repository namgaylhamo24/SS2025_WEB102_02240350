
# Main Concepts Applied

1. **RESTful API Design**:
   - Created resource-based endpoints, in accordance with the REST principles.
   - Employed the appropriate HTTP request methods (GET, POST, PUT, DELETE) to satisfy CRUD operations.
   - Nested routing design for relations (e.g., `/videos/:id/comments`).
   - Supported pagination and filtering.

2. **Social Media Features**:
   - Developed core TikTok-like features (videos, comments, likes).
   - Developed user relationship features (following/followers).
   - Developed engagement metrics (likes on videos and comments).

3. **Middleware Architecture**:
   - Setting essential middleware (CORS, logging, body parsing).
   - Content negotiation implemented (JSON-only response).
   - Added error handling middleware.

4. **Data Management**:
   - Built an in-memory data store with relationships.
   - Data validation and consistency were checked.
   - Delete cascades: deleting a user deletes all of his content.

5. **API Testing**:
   - Provided sample testing commands via cURL.
   - Covered error returns.
   - Use appropriate status codes (200, 201, 400, 404, etc.).

### What I Learned

1. **Complex Relationship Handling**:
   - Gained experience managing many-to-many relationships (followers/following)
   - Learned to maintain data consistency across related resources
   - Implemented proper validation for relationship operations

2. **Social Media Logic**:
   - Understood the backend logic behind engagement features (likes, comments)
   - Learned to prevent duplicate engagements (preventing duplicate likes)
   - Implemented self-reference checks (users can't follow themselves)

3. **API Design Patterns**:
   - Practiced nested routing for related resources
   - Learned to structure endpoints for complex applications
   - Implemented consistent response formats

4. **Error Handling**:
   - Created specific error responses for different failure cases
   - Learned to handle concurrent data modification scenarios
   - Implemented proper HTTP status codes for social media scenarios

### Challenges Faced and Solutions

1. **Circular Dependencies in Relationships**:
   - Challenge: Maintaining consistency between followers/following lists
   - Solution: Implemented bidirectional updates in a single transaction
   ```javascript
   // In followUser controller
   userToFollow.followers.push(followerIdInt);
   follower.following.push(userToFollowId);
   ```

2. **Cascading Deletions**:
   - Challenge: Properly cleaning up related data when a user is deleted
   - Solution: Implemented comprehensive cleanup logic
   ```javascript
   // In deleteUser controller
   dataStore.videos = dataStore.videos.filter(v => v.userId !== userId);
   dataStore.comments = dataStore.comments.filter(c => c.userId !== userId);
   ```

3. **Like/Unlike Validation**:
   - Challenge: Preventing duplicate likes and validating unlike operations
   - Solution: Added existence checks before operations
   ```javascript
   // In likeVideo controller
   if (video.likes.includes(userIdInt)) {
     return res.status(409).json({ error: 'User already liked this video' });
   }
   ```

4. **Data Consistency**:
   - Challenge: Maintaining referential integrity in memory
   - Solution: Created comprehensive validation checks
   ```javascript
   // In createVideo controller
   const userExists = dataStore.users.some(user => user.id === parseInt(userId));
   if (!userExists) {
     return res.status(400).json({ error: 'User does not exist' });
   }
   ```

5. **Error Response Design**:
   - Challenge: Creating helpful error messages for API consumers
   - Solution: Standardized error response format
   ```javascript
   {
     "error": "Descriptive error type",
     "message": "Detailed explanation for developers"
   }
   ```

### Key Takeaways

This project provided valuable experience in building a complete social media backend API. The implementation of complex user relationships and engagement features gave me deeper insight into real-world API development challenges.

Key lessons learned:
1. Relationship management requires careful transaction handling
2. Comprehensive validation is essential for data integrity
3. Clear error messaging significantly improves API usability
4. Nested routes provide intuitive API structure for related resources
5. In-memory data stores work for prototyping but have limitations

Areas for future improvement:
1. Add authentication and authorization
2. Implement rate limiting
3. Add pagination to list endpoints
4. Include more comprehensive documentation (Swagger/OpenAPI)
5. Transition to a proper database system

This practical exercise has given me confidence in designing and implementing complex REST APIs with multiple interrelated resources, preparing me for real-world backend development challenges.
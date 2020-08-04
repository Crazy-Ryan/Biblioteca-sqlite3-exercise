## Answer to the sqlite3 exercise (Biblioteca Release 3)

1. Who checked out the book 'The Hobbit'?

   ```sqlite
   SELECT member.name
   FROM member,
        book,
        checkout_item
   WHERE checkout_item.book_id == book.id
     AND checkout_item.member_id == member.id
     AND book.title == 'The Hobbit';
   ```

2. How many people have not checked out anything?

   ```sqlite
   SELECT count(member.id)
   FROM member
   WHERE id NOT IN (SELECT member_id FROM checkout_item);
   ```

3. What books and movies aren't checked out?

   ```sqlite
   SELECT *
   FROM book
   WHERE id NOT IN (SELECT book_id FROM checkout_item WHERE book_id IS NOT NULL);
   ```

4. Add the book 'The Pragmatic Programmer', and add yourself as a member. Check out 'The Pragmatic Programmer'. Use your query from question 1 to verify that you have checked it out. Also, provide the SQL used to update the database.

   ```sqlite
   INSERT INTO book (id, title)
   VALUES (11, 'The Pragmatic Programmer');
   
   INSERT INTO member(id, name)
   VALUES (43, 'Ran Wang');
   
   INSERT INTO checkout_item(member_id, book_id)
   VALUES (43, 11);
   
   SELECT member.name
   FROM member,
        book,
        checkout_item
   WHERE checkout_item.book_id == book.id
     AND checkout_item.member_id == member.id
     AND book.title == 'The Pragmatic Programmer';
   ```

5. Who has checked out more than 1 item?

   ```sqlite
   SELECT name
   FROM member,
        (
            SELECT member_id, (count(book_id) + count(movie_id)) AS item_count FROM checkout_item GROUP BY member_id)
   WHERE item_count >= 2
     AND member.id == member_id;
   ```

   


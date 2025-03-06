import json
import os

BOOKS_FILE = "books.json"

def load_books():
    """Load books from the JSON file."""
    if not os.path.exists(BOOKS_FILE):
        return []
    with open(BOOKS_FILE, "r") as file:
        try:
            return json.load(file)
        except json.JSONDecodeError:
            return []

def save_books(books):
    """Save books to the JSON file."""
    with open(BOOKS_FILE, "w") as file:
        json.dump(books, file, indent=4)

def add_book():
    """Add a new book to the store."""
    books = load_books()
    
    title = input("Enter book title: ")
    author = input("Enter book author: ")
    isbn = input("Enter book ISBN: ")
    genre = input("Enter book genre: ")
    
    try:
        price = float(input("Enter book price: "))
        if price <= 0:
            raise ValueError
        quantity = int(input("Enter book quantity in stock: "))
        if quantity < 0:
            raise ValueError
    except ValueError:
        print("Invalid price or quantity. Please enter valid numbers.")
        return
    
    for book in books:
        if book["isbn"] == isbn:
            print("A book with this ISBN already exists!")
            return
    
    books.append({
        "title": title,
        "author": author,
        "isbn": isbn,
        "genre": genre,
        "price": price,
        "quantity": quantity
    })
    
    save_books(books)
    print("Book added successfully!")

def view_books():
    """Display all books in a neat format."""
    books = load_books()
    if not books:
        print("No books available.")
        return
    
    print("\nAvailable Books:")
    print("=" * 60)
    for book in books:
        print(f"Title: {book['title']}, Author: {book['author']}, ISBN: {book['isbn']}, Genre: {book['genre']}, Price: ${book['price']}, Stock: {book['quantity']}")
    print("=" * 60)

def search_book():
    """Search for a book by title, author, or ISBN."""
    books = load_books()
    query = input("Enter title, author, or ISBN to search: ").lower()
    
    found_books = [book for book in books if query in book['title'].lower() or query in book['author'].lower() or query in book['isbn'].lower()]
    
    if not found_books:
        print("No books found matching the search query.")
        return
    
    print("\nSearch Results:")
    print("=" * 60)
    for book in found_books:
        print(f"Title: {book['title']}, Author: {book['author']}, ISBN: {book['isbn']}, Genre: {book['genre']}, Price: ${book['price']}, Stock: {book['quantity']}")
    print("=" * 60)

def remove_book():
    """Remove a book using its ISBN."""
    books = load_books()
    isbn = input("Enter ISBN of the book to remove: ")
    
    new_books = [book for book in books if book['isbn'] != isbn]
    
    if len(new_books) == len(books):
        print("No book found with that ISBN.")
        return
    
    save_books(new_books)
    print("Book removed successfully!")

def main():
    """Main menu system."""
    while True:
        print("\nBook Store Management System")
        print("1. Add Book")
        print("2. View Books")
        print("3. Search Book")
        print("4. Remove Book")
        print("5. Exit")
        
        choice = input("Select an option: ")
        if choice == "1":
            add_book()
        elif choice == "2":
            view_books()
        elif choice == "3":
            search_book()
        elif choice == "4":
            remove_book()
        elif choice == "5":
            print("Exiting program. Data saved.")
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()

# Library Management System (Qt Widgets, C++)

This project implements the COS2614 Assignment 3 brief:

- OOP with base/derived classes and polymorphism
- Qt containers (`QList`), sorting/filtering, and model/view with `QListView + QSortFilterProxyModel`
- A **custom Qt static library** (`LibraryUtils`) for persistence and helpers
- A **template (generics)** class `Storage<T>`
- Qt Widgets GUI (add/search/borrow/return)
- File persistence to `library_data.txt` in the app directory

## Project layout

```
LibraryManagementSystem/
├─ LibraryManagementSystem.pro     # SUBDIRS project
├─ libraryutils/
│  ├─ libraryutils.pro             # static lib
│  ├─ libraryutils.h               # save/load/sort/filter + Storage<T> template
│  ├─ libraryutils.cpp
│  └─ storage.h                    # includes template for marker visibility
└─ app/
   ├─ app.pro                      # GUI app
   ├─ main.cpp
   ├─ mainwindow.h/.cpp
   ├─ libraryitem.h/.cpp
   ├─ book.h/.cpp
   └─ magazine.h/.cpp
```

## Build & Run (Qt Creator)

1. Open `LibraryManagementSystem.pro` (the SUBDIRS project) in Qt Creator.
2. Configure a Desktop kit (Qt 5.15+ or Qt 6.x) and run **Build All**.
3. Ensure **the library** builds first (Qt’s SUBDIRS does this automatically via `app.depends = libraryutils`).
4. Run the `LibraryManagementSystem` target.

## Usage

- Enter Title, Author, and ID.
- For a **Book**, also set **Genre**, then click **Add Book**.
- For a **Magazine**, set **Issue#**, then click **Add Magazine**.
- Use **Search** to filter by Title/Author (case-insensitive).
- Use **Sort by** to sort Title/Author/ID.
- Select an item and click **Borrow** or **Return** to toggle status.
- Data persists to `library_data.txt` in the executable’s directory.

## Marking rubric mapping

- **Class Design (10):** `LibraryItem` (encapsulation, virtual `displayInfo()`), derived `Book` and `Magazine`.
- **Qt Lists & Containers (20):** `QList<LibraryItem*> items_`, sorting via utility, filtering via proxy. Display via `QListView` (also `QStandardItemModel`).
- **Custom Qt Library (20):** `libraryutils` static lib with save/load/sort/filter used by the app.
- **Generics (10):** `Storage<T>` template used in `MainWindow` (`storage_.add(it)`).
- **GUI (30):** Built with `QMainWindow` + layouts + inputs + buttons + `QListView`.
- **File Handling (10):** Save/Load with `QFile`/`QTextStream` to `library_data.txt`.

## Notes

- Memory: App owns `items_` and deletes them on exit (`qDeleteAll`).
- The model stores raw pointers as `qulonglong` for simplicity; in production, consider smart pointers and safer indirection.
- The static library references app headers just for the concrete `deserialize`/`serialize` usage; for a stricter boundary, introduce pure-virtual factory interfaces.

# اتصال به دیتابیس SQLite در برنامه‌های NextJS


برای اتصال موفق به دیتابیس SQLite در برنامه‌های NextJS کافیست تا گام‌های زیر را طی کنید:

1) با اجرای دستور زیر، ماژول‌های مورد نیاز را نصب کنید:

```
npm install sqlite sqlite3
```

2) در مسیر اصلی پروژه، یک دایرکتوری به نام `db` (یا هر نام دلخواه دیگری) ایجاد کنید.

3) در مسیر `pages/api` یک فایل به نام `database.js` ایجاد کنید و قطعه کد زیر را درون آن، قرار دهید:

```js
import sqlite3 from 'sqlite3';
import { open } from 'sqlite';
import path from 'path';

export default async function handler(req, res) {
  const dbPath = path.join(process.cwd(), 'db', 'mydatabase.db');
  const dbExists = await checkDatabaseExists(dbPath);

  if (!dbExists && req.method !== 'POST') {
    return res.status(200).json({ message: 'Database not created yet', users: [] });
  }

  if (req.method === 'POST' && !dbExists) {
    await createDatabase(dbPath);
  }

  if (req.method === 'GET') {
    const users = await getUsersFromDatabase(dbPath);
    res.status(200).json({ users });
  } else if (req.method === 'POST') {
    const { name, email } = req.body;
    if (!name || !email) {
      return res.status(400).json({ message: 'Name and email are required' });
    }

    try {
      const db = await open({
        filename: dbPath,
        driver: sqlite3.Database
      });
      await db.exec(`
        CREATE TABLE IF NOT EXISTS users (
          id INTEGER PRIMARY KEY AUTOINCREMENT,
          name TEXT NOT NULL,
          email TEXT NOT NULL
        )
      `);
      await db.run('INSERT INTO users (name, email) VALUES (?, ?)', [name, email]);
      await db.close();
      res.status(200).json({ message: 'Data added successfully' });
    } catch (error) {
      console.error('Error adding data:', error.message);
      res.status(500).json({ message: 'Internal Server Error' });
    }
  } else {
    res.status(405).json({ message: 'Method Not Allowed' });
  }
}

async function checkDatabaseExists(dbPath) {
  const fs = require('fs');
  return fs.existsSync(dbPath);
}

async function createDatabase(dbPath) {
  const db = await open({
    filename: dbPath,
    driver: sqlite3.Database
  });

  try {
    await db.exec(`
      CREATE TABLE IF NOT EXISTS users (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT NOT NULL,
        email TEXT NOT NULL
      )
    `);
  } catch (error) {
    console.error('Error creating database:', error.message);
  } finally {
    await db.close();
  }
}

async function getUsersFromDatabase(dbPath) {
  const db = await open({
    filename: dbPath,
    driver: sqlite3.Database
  });

  try {
    const users = await db.all('SELECT * FROM users');
    return users;
  } catch (error) {
    console.error('Error fetching users:', error.message);
    return [];
  } finally {
    await db.close();
  }
}
```

4) در فایل `pages/index.js` قطعه کد زیر را قرار دهید:

```js
import { useState, useEffect } from 'react';

export default function Home() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  const [message, setMessage] = useState('');
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetchUsers();
  }, []);

  const fetchUsers = async () => {
    const response = await fetch('/api/database');
    const data = await response.json();
    setUsers(data.users);
  };

  const handleAddData = async (event) => {
    event.preventDefault();
    const response = await fetch('/api/database', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ name, email }),
    });
    const data = await response.json();
    setMessage(data.message);
    fetchUsers();
    setName('');
    setEmail('');
  };

  return (
    <div>
      <h1>SQLite Database Operations</h1>
      <form onSubmit={handleAddData}>
        <label>
          Name:
          <input
            type="text"
            value={name}
            onChange={(e) => setName(e.target.value)}
          />
        </label>
        <label>
          Email:
          <input
            type="email"
            value={email}
            onChange={(e) => setEmail(e.target.value)}
          />
        </label>
        <button type="submit">Add Data</button>
      </form>
      {message && <p>{message}</p>}
      <h2>Users</h2>
      <ul>
        {users.map((user, index) => (
          <li key={index}>
            <strong>Name:</strong> {user.name}, <strong>Email:</strong> {user.email}
          </li>
        ))}
      </ul>
    </div>
  );
}
```


5) در بخش [**دیسک‌ها**](../../../../disks/about.md) برنامه خود در لیارا، یک دیسک جدید با نام `database` و اندازه مدنظرتان ایجاد کنید.

6) در مسیر اصلی پروژه، یک فایل به نام `liara.json` ایجاد کنید و قطعه کد زیر را، درون آن، قرار دهید:

```json
{
    "disks": [
        {
            "name": "database",
            "mountTo": "db"
        }
    ]
}
```

7) برنامه خود را با استفاده از دستور `liara deploy` در لیارا مستقر کنید.

> [!NOTE]
> دستورات فوق را می‌توانید بنا به نیاز خود، شخصی‌سازی کنید. 

> [!TIP]
> یک پروژه آماده استقرار در [اینجا]() قرار دارد که می‌توانید از آن استفاده کنید.
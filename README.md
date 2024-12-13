# API SPESIFICATIONS

API ini menyediakan berbagai endpoint untuk menghubungkan mobile apps ke database.

## BASE URL

```
http://localhost:8000/api
```

## RESOURCES

### 1. Authentication

#### a. Login Users

-   **Endpoint** : `/login`
-   **Method**: `POST`
-   **Description**: Login users pada platform
-   **Request:**

```json
{
    username: number,
    password: string
}
```

-   **Response**:

```json
{
    "status": 200,
    "message": "Login users successfully",
    "data": {
        "token": "token",
        "role": "mahasiswa",
        "users": {
            "name": "Maulana Malik Ibrahim",
            "username": "mallexibra",
            "telp": "082342374",
            "alamat": "Banyuwangi",
            "image": "http://localhost:8000/imageprofile.jpg",
            "semester": 3,
            "prodi": "Teknologi Rekayasa Perangkat Lunak"
        }
    }
}
```

#### b. Forgot Password

-   **Endpoint:** `/send-email-verification`
-   **Method:** `POST`
-   **Description:** Lupa password mengirimkan kode ke email
-   **Request:**

```json
{
    email: string
}
```

-   **Response**:

```json
{
    "status": 200,
    "message": "Check your email for code verification"
}
```

#### c. Email Verification

-   **Endpoint:** `/forgot-password`
-   **Method:** `POST`
-   **Description:** Verifikasi kode dari email
-   **Request:**

```json
{
    email: string
    verification_code: number
}
```

-   **Response:**

```json
{
    "status": 200,
    "message": "Email Verification Successfully"
}
```

#### d. Reset Password

-   **Endpoint:** `/reset-password`
-   **Method:** `POST`
-   **Description:** Reset password pengguna setelah berhasil mengirimkan kode
-   **Request:**

```json
{
    new_password: string
}
```

-   **Response:**

```json
{
    "status": 200,
    "message": "Reset password successfully"
}
```

#### e. Logout User

-   **Endpoint:** `/logout`
-   **Method:** `POST`
-   **Description:** Logout user dari aplikasi
-   **Response:**

```json
{
    "status": 200,
    "messagge": "Logout user successfully"
}
```

### 2. Dashboard

#### a. Get Data Dashboard

-   **Endpoint:**: `/student-dashboard`
-   **Method:** `GET`
-   **Description:** Menampilkan data untuk dashboard mahasiswa
-   **Response:**

```json
{
    "status": 200,
    "message": "Get data student dashboard successfully",
    "data": {
        "number_of_tasks": 10,
        "number_of_requests": 2,
        "number_of_rejected_requests": 3,
        "number_of_finished_requests": 4,
        "number_of_proccess_requests": 1,
        "data_tasks": [
            {
                id: 1,
                title: "Tugas 1",
                description: "Tugas 1 silahkan dikerjakan",
                type_of_assignment: "file" // bisa file atau url
                deadline: "17-10-2024 09:00"
            }
        ]
    }
}
```

### 3. Activities

#### a. Get Data Requests Unfinished

-   **Endpoint:** `/requests-unfinished`
-   **Method:** `GET`
-   **Description:** Menampilkan data request yang belum selesai
-   **Response**:

```json
{
    status: 200,
    message: "Get data requests unfinished",
    data: {
        number_of_unfinished_requests: 10,
        unfinished_requests: [
            {
                id: 1,
                task_title: "Tugas 1",
                task_description: "Tugas 1 silahkan dikerjakan",
                acc_dosen: "terima", // bisa null, terima dan tolak
                acc_kaprodi: null,
                download_kompensasi: null, // link untuk download kompensasi
                comments: [
                    {
                        id: 1,
                        id_user: 2,
                        name: "Hanif",
                        image: "http://localhost:8000/profile.jpg"
                        comment: "Keren sekali"
                    },
                    {
                        id: 2,
                        id_user: 2,
                        name: "Hanif",
                        image: "http://localhost:8000/profile.jpg"
                        comment: "Keren banget"
                    }
                ]
            }
        ]
    }
}
```

### 4. History

#### a. Get Data Requests Finished

-   **Endpoint:** `/requests-finished`
-   **Method:** `GET`
-   **Description:** Menampilkan data request yang telah selesai
-   **Response:**

```json
{
    status: 200,
    message: "Get data requests finished",
    data: {
        number_of_finished_requests: 10,
        finished_requests: [
            {
                id: 1,
                task_title: "Tugas 1",
                task_description: "Tugas 1 silahkan dikerjakan",
                acc_dosen: "terima",
                acc_kaprodi: "terima",
                download_kompensasi: "http://localhost:8000/api/generate-pdf/1?download=true",
                comments: [
                    {
                        id: 1,
                        id_user: 2,
                        name: "Hanif",
                        image: "http://localhost:8000/profile.jpg"
                        comment: "Keren sekali"
                    },
                    {
                        id: 2,
                        id_user: 2,
                        name: "Hanif",
                        image: "http://localhost:8000/profile.jpg"
                        comment: "Keren banget"
                    }
                ]
            }
        ]
    }
}
```

### 5. Notifications

#### a. Get Data Notifications

-   **Endpoint:** `/notifications`
-   **Method:** `GET`
-   **Description:** Menampilkan data notifications dari status yang diperbarui
-   **Response:**

```json
{
    "status": 200,
    "message": "Get data notifications successfully",
    "data": [
        {
            "id": 1,
            "title": "Tugas mu sudah dikumpulkan",
            "time": "17-10-2024 09:00" // ambil dari created_at diformat diffForHumans
        }
    ]
}
```

### 6. Tasks

#### a. Get data Task

-   **Endpoint:** `/tasks`
-   **Method:** `GET`
-   **Description:** Menampilkan data tugas yang tersedia
-   **Response:**

```json
{
    status: 200,
    message: "Get data tasks successfully",
    data: [
        {
            id: 1,
            title: "Tugas 1",
            description: "Tugas 1 silahkan dikerjakan",
            type_of_assignment: "file" // bisa file atau url
            deadline: "17-10-2024 09:00"
        }
    ]
}
```

#### b. Post data submission

-   **Endpoint:** `/submission-task`
-   **Method:** `POST`
-   **Description:** Mengirimkan data jawaban dari tugas
-   **Request:**

```json
{
    id_task: number,
    assignment: (binary) // bisa dalam bentuk file atau url
}
```

-   **Response:**

```json
{
    "status": 200,
    "message": "Post data task submission successfully"
}
```

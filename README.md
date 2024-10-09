# bibliotecaDB
Hola profe, en el readme te dejo las consultas por que no estoy seguro si al exportar las colecciones solo son los atributos o todo 
att: Juan David Calderon Jaramillo

 // Eliminar un libro
db.books.deleteOne({ _id: ObjectId("641e7d9d14c0b9d8e947fa0e") });

// Parte 2: Consultas Avanzadas
// Libros Disponibles por Género
const genre = "Ficción";
db.books.find({ genres: genre, available_copies: { $gt: 0 } });

// Usuarios con Libros Prestados
db.users.find({ "borrowed_books.0": { $exists: true } });

// Historial de Préstamos por Usuario
const userId = ObjectId("641e7e5f14c0b9d8e947fa10");
db.loans.find({ user_id: userId });

// Libros Más Prestados
db.loans.aggregate([
    { $group: { _id: "$book_id", count: { $sum: 1 } } },
    { $sort: { count: -1 } },
    { $lookup: {
        from: "books",
        localField: "_id",
        foreignField: "_id",
        as: "book_info"
    }},
    { $unwind: "$book_info" },
    { $project: { title: "$book_info.title", count: 1 } }
]);

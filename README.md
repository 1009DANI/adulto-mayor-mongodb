# adulto-mayor-mongodb
MongoDB
# INSERTAR
db.beneficiarios.insertOne({
  codigo_dane: 5664,
  vigencia: 2025,
  edad: 70,
  grupo_del_ciclo_de_vida: "Vejez",
  grupos_de_edad: "70-79 años",
  sexo: "M",
  patologias: "Ninguna",
  alguna_pat_ninguna_nr: "Ninguna",
  eps: "SURA",
  regimen: "CONTRIBUTIVO",
  discapacidad: "SIN DISCAPACIDAD",
  victima: "NO VÍCTIMA",
  clasificacion_del_sisben: "C4",
  zona: "URBANA"
})
# SELECCIONAR
# mostrar todo
db.beneficiarios.find()
# mostrar campos especificos
db.beneficiarios.find(
  {},
  { _id: 0, edad: 1, sexo: 1, eps: 1, zona: 1 }
)
# ordenar por edad
db.beneficiarios.find().sort({ edad: -1 })
# ACTUALIZAR
db.beneficiarios.updateOne(
  { edad: 83 },
  { $set: { zona: "RURAL" } }
)
# incrementar edad
db.beneficiarios.updateOne(
  { sexo: "F" },
  { $inc: { edad: 1 } }
)
# ELIMINAR
db.beneficiarios.deleteOne({ edad: 90 })
# CONSULTAR BFILTROS NY OPERACIONES
db.beneficiarios.find({ edad: { $gt: 75 } })
db.beneficiarios.find({
  sexo: "F",
  edad: { $gte: 60, $lte: 70 }
})
db.beneficiarios.find({ eps: "NUEVA EPS" })
db.beneficiarios.find({ alguna_pat_ninguna_nr: "Alguna Pat" })
# CONSULTAS DE AGREGACION
db.beneficiarios.aggregate([
{ $group: { _id: "$sexo", total: { $sum: 1 } } }
])
db.beneficiarios.aggregate([
  { $project: { patologias: { $split: ["$patologias","-"] } } },
  { $unwind: "$patologias" },
  { $group: { _id: { $trim: { input: "$patologias" } }, total: { $sum: 1 } } },
  { $sort: { total: -1 } },
  { $limit: 5 }
])



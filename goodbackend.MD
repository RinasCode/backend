                                                           KITAB PEMBUATAN BACK END
								
# PERTAMA LAKUKAN INSTALASI YANG DIPERLUKAN 

1. nmp init -y >>>>>>> hasilnya package json
2. npm i express sequelize pg (hasilnya node modules,package json-lock) 
apa bedanya package dan framework ?
package >>>> koleksi library yang dikelola bersama-sama dan didistribusikan sebagai satu kesatuan. 
Package dapat mencakup satu atau lebih library serta dependensi lain yang diperlukan.
framework >>>>  struktur atau kerangka kerja yang lebih besar yang menyediakan fondasi dan pola desain untuk pengembangan aplikasi. 
buat apa si mereka ini ? 
    "axios": <<<<< sebuah library JavaScript yang digunakan untuk membuat HTTP requests dari aplikasi JavaScript, 
	               baik di browser maupun di Node.js. kalo pakai 3rd party API.
    "bcryptjs": <<<< sebuah library untuk hashing password dan menjaga keamanan data sensitif
    "cloudinary": <<<< 3rd party untuk upload file
    "cors": <<<< CORS (Cross-Origin Resource Sharing) adalah mekanisme yang memungkinkan server untuk menentukan dan mengontrol apakah permintaan dari domain yang berbeda (origin) diizinkan atau tidak.
	             pas buat pakai local host 3000 pas deploy pakai 80. kasih perizinan dengan cors biar bisa di pakai keduanya.
    "express": >>>> framework yang digunakan untuk membangun aplikasi web tugasnya (routing, middleware, dan rendering)
    "jsonwebtoken": <<<< library JavaScript yang digunakan untuk membuat, menandatangani, dan memverifikasi JSON Web Tokens (JWT).
	                     client dibuatkan token JWT, server bisa cek untuk validasi dengan secret key.
    "multer": <<<< middleware untuk Node.js yang digunakan dengan Express.js untuk menangani file upload dari form HTML. untuk menyimpannya ke cloud storage.
	               multer menyediakan tempat penyimpanan untuk 3rd party upload file seperti cloudinary atau imagekit.
    "pg": >>>>  sistem manajemen basis data relasional (RDBMS).untuk menghubungkan aplikasi Node.js ke database PostgreSQL, menjalankan query, dan mengelola data.
    "sequelize": >>>> ORM yang digunakan agar kita tidak perlu menulis query SQL secara langsung. sequelize adlaah ORM (Object-Relational Mapping) 
	 untuk Node.js yang mendukung berbagai database SQL seperti MySQL, PostgreSQL, SQLite, dan MSSQL.
3. npm i axios bcryptjs cloudinary cors jsonwebtoken multer (install sesuai kebutuhan soal)
4. npm i -D nodemon sequelize-cli dotenv (hasilnya config,migrate,model,seeders,node_modules)
   kenapa npm i-D (development dependency). Paket-paket ini diperlukan selama fase pengembangan dan pengujian aplikasi tetapi tidak diperlukan dalam lingkungan produksi.
   tambahkan ini sesuai kebutuhan ya.
	"dotenv": <<<< sebuah library untuk Node.js yang digunakan untuk memuat variabel lingkungan (environment variables) dari file .env ke dalam process.env. bisa atur mana yang ada di dev dan prodc.
    "jest": <<<< Untuk testing.sebuah framework pengujian (testing framework) untuk JavaScript yang dikembangkan oleh Facebook
    "nodemon": >>>> buat jalanin aplikasi ebuah alat pengembangan untuk Node.js yang secara otomatis me-restart aplikasi Node.js ketika ada perubahan pada file dalam proyek.
    "sequelize-cli": >>>> buat kelola/ akses database. lat baris perintah (command-line tool) yang digunakan bersama Sequelize, yaitu ORM (Object-Relational Mapping) untuk Node.js.
    "supertest": <<<< Untuk testing. sebuah library untuk Node.js yang digunakan untuk menguji aplikasi HTTP, khususnya aplikasi Express.
	=========================================================================================================================================================================================================
	npx sequelize-cli init
5. touch .gitignore
6. isi config dengan node_modules dan .env 



# KEDUA SIAPKAN TEMPAT DATA KITA DENGAN BUAT NAMA DATABASE, MODEL DAN SEED DATA 

1. npx sequelize-cli db:create
2. buat model =====> npx sequelize-cli model:generate --name NamaModel --attributes name:tipe(string/integer/date/text)
   BUATLAH MODEL YANG TIDAK ADA FKNYA DULU YA.JANGAN BUAT MODEL TABEL KONJUNGSI NANTI GAGAL. BUAT MODEL YANG BERDIRI SENDIRI DULU.
3. isi model dan juga migration.
   Gimana cara isinya ?
   >>>> model dan migrasi sama sama di isi dengan (type dan allownull false)
   >>>> model isi dengan (validate) lihat soal mana aja yang perlu validate.
        validate itu apa aja ? VALIDASI PADA MODEL
		1. paling umum ga boleh kosong dan ga boleh null
		validate : {
        notEmpty : {
          msg : "Title is required"
        },
        notNull : {
          msg : "Title is required"
        }
        }
		2. cek apakah ini email (masukan ke dalam obj validate)
		isEmail : {
        msg : "Invalid email"
        }
		3. tentukan panjang minimal password (masukan ke dalam obj validate)
		len :{
          args : [5],
          msg : "Minimum password characters is 5"
        }
		-----------------------------------------------------------------------------------------------------------
		kalo ada default value gimana ? 
		defaultValue : "isidefaultvaluenya" (taro dibawah allownull false atau setara/ga boleh masuk dalam validate.
		-----------------------------------------------------------------------------------------------------------
		assosiasinya gimana ?
		1. cari tulisan ini di model  static associate(models){} isi di dalam obj nya.
		2. isinya apa ? tergantung dari hubungannya 
		   ingat asosiasi itu cuma 3 ( one to one, one to many, dan many to many)
		   paling sering itu one to many dan many to many. bukti udah terasosiasi ada di Dbeaver lihat garis assosiasinya.
		    >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> Many to  Many >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
		    1. buka model yang sudah di buat.
			   - isi bagaian init,semuanya dipakaikan type dan allownull:false.
			   - kalo ada email isi yg ada unique:true,
               - kalo ada defaultvalue:'isi dengan dafaultvaluenya'
               - kalo minta validasi isi dengan panduan VALIDASI PADA MODEL di atas.
			   - isi hubungan assosiasinya :
			      --- pastiin di file model, cari tulisan static associate(models)
			      --- yang punya banyak tulisnya gini (biasanya ada 2)
				       Course.belongsToMany(models.User,{through:models.MyCourse}) ===> NamaModel dan konjungsi.
					   User.belongsToMany(models.User, {through:models.MyCourse})  ===> NamaModel dan konjungsi.
				  --- yang jadi tabel konjungsi tulisnya gini 
				       MyCourse.belongsTo(models.User) ===> NamaModel dan NamaModel
                       MyCourse.belongsTo(models.Course) ===> NamaModel dan NamaModel
            2. buka file migrasi :
               - isi semua dengan allownull:false kecuali defaultValue:'isi dengan dafaultvalue'
               - isi bagian email dengan unique:true
               - pada bagian foreignKey isi dengan referance :{
                                                      model : 'Namamodel dengans',
                                                      key   : 'id'			
                                                    },
               - jangan lupa isi OnuUpdate dan OnDelete. isinya 'CASCADE'
			3. kalo sudah semua bisa lanjut =====> npx sequelize-cli db:migrate
			
			>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> One to Many >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
  			 1. buka model yang sudah di buat.
			   - isi bagaian init,semuanya dipakaikan type dan allownull:false.
			   - kalo ada email isi yg ada unique:true,
               - kalo ada defaultvalue:'isi dengan dafaultvaluenya'
               - kalo minta validasi isi dengan panduan VALIDASI PADA MODEL di atas.
			   - isi hubungan assosiasinya :
			      --- pastiin di file model, cari tulisan static associate(models)
			      --- yang punya banyak tulisnya gini (biasanya ada 2)
				       Category.hasMany(models.Article, {foreignKey: "categoryId"}) ===> NamaModel ada 2 dan ada FK. (hasMany)---hasmany pada model yang sama
					   User.hasMany(models.Article, {foreignKey: "authorId"})       ===> NamaModel ada 2 dan ada FK. (hasMany)---hasmany pada model yang sama
				  --- yang DIPUNYAIN BANYAK (MILIK BERSAMA) tulisnya gini
				       Article.belongsTo(models.User, { foreignKey: "authorId" })    ====> NamaModel ada 2 dan ada FK. (belongsTo)---belongsTo pada model yang sama
                       Article.belongsTo(models.Category, { foreignKey: "categoryId" }) ====> NamaModel ada 2 dan ada FK. (belongsTo)---belongsTo pada model yang sama
            2. buka file migrasi :
               - isi semua dengan allownull:false kecuali defaultValue:'isi dengan dafaultvalue'
               - isi bagian email dengan unique:true
               - pada bagian foreignKey isi dengan referance :{
                                                      model : 'Namamodel dengans',
                                                      key   : 'id'			
                                                    },
               - jangan lupa isi OnuUpdate dan OnDelete. isinya 'CASCADE'
			3. kalo sudah semua bisa lanjut =====> npx sequelize-cli db:migrate
4. isi data dengan seeding gimana caranya ?
5. buat file seeding dulu. *SEEDING FILE YANG BERDIRI SENDIRI DAHULU*
   - syntaxnya ====> npx sequelize-cli seed:generate --name namafileseed
   - buat sesuai dengan kebutuhan mau seeding apa aja 
6. buat folder data yang isinya file data dalam bentuk json.
7. buat folder helper yang isinya bcrypt.js 
                    isi dari file bcrypt seperti ini 
					
					const bcrypt = require('bcryptjs');

                    function hash(pass) {
                         const salt = bcrypt.genSaltSync(10);
                         return bcrypt.hashSync(pass, salt)
                    }

                    function comparePassword(pass, hashedPass) {
                         return bcrypt.compareSync(pass, hashedPass)
                    }

                    module.exports = { hash, comparePassword }

8. seeding data user(yang perlu hash password) bagaimana caranya ?
   - pastiin udah install bcryptjs. kalo belum ? npm i bcryptjs
   - import dulu si bcrypt karena dia yang punya tugas untuk hash pasword. const {hash} = require('../helpers/bcrypt')
   - setelah itu baca data seperti biasa : 
     baca data seperti biasa itu kaya gini :
	  ---  const namavarbebas = require('path data json').map((e) => {
           delete e.id
           e.createdAt = e.updatedAt = new Date ()
           return e
           })
           await queryInterface.bulkInsert('namaModelpakai'S',namavarbebas,{})----up 
		   await queryInterface.bulkDelete('Users',null,{}) ---- down
	 baca data saat ada hash password gimana ?
	 ----  e.password=hash(e.password) ---tinggal tambahin ini setelah e.id
   - data yang lain di baca dengan cara biasa.
9.  SEED SATU DATA aja npx sequelize-cli db:seed --seed 20240727064510-category.js --- nama file, rename , lalu copas ke terminal maka seed cuma file yang di tuju.
10. SEED SEMUA ---  npx sequelize db:seed:all
11. kalo salah seeding semua gimmana ? --- pakai ini npx sequelize db:seed:undo:all 
12. kalo mau undo ke file tertentu aja gimana ? ---- pakai ini npx sequelize-cli db:seed:undo --seed 20240727064510-category.js --- nama file, rename , lalu copas ke terminal maka seed cuma file yang di tuju.
13. saat ini data sudah siap di olah. 
    --- seed  bisa langsung di filenya ? iya bisa tapi lebih baik data di pisah di file sendiri agar rapih ga nyampur kode.


# BUAT CONTROLLER, APP.JS ATAU ROUTER 
1. buat folder controllers isinya controller.js sesuai perintah soal.
   bagaimana cara buat controller ? 
   controller ada 2 yang full CRUD(Model) dan AUTH (USER)
   --- controller AUTH
   1. buat file jwt.js pada folder helpers.
      isinya ini :  
	  const jwt = require('jsonwebtoken')

      const signToken = (payload) => {
      return jwt.sign(payload, "inigaaman")
      }

      const verifyToken = (token) => {
      return jwt.verify(token, "inigaaman")
      }

      module.exports = { signToken, verifyToken }
   2. const model User.
   3. const compare dari bcrypt.
   4. const signToken dari jwt.
   5. buat class AuthController
   6. module.export class AuthController.
   7. buat 2 static async Register dan login.
   8. * async register 
      - isi try dengan :
      - const { email, password } = req.body
        const varbebas = await Model.create({ email, password })

        res.status(201).json({
           id: varbebas.id,
           email: varbebas.email
        })
	  - isi catch dengan :
	    console.log, next jika sudah buat eror handler.
	  * async login 
	  - isi try dengan :
	  - const { email, password } = req.body

        if (!email) throw { name: "EmailReq" }
        if (!password) throw { name: "PassReq" }

        const user = await User.findOne({
            where: {
                email
            }
        })

        if (!user) throw { name: "LoginError" }

        if (!compare(password, user.password)) throw { name: "LoginError" }

        const payload = {
            id: user.id,
            email: user.email
        }

        const access_token = signToken(payload)

        res.status(200).json({
            access_token
        })
	   - isi catch dengan :
	    console.log, next jika sudah buat eror handler.
		
   
   --- controller CRUD 
       1. require model
	      const {namaModel} = require ('../models')
	   2. buat class ModelController
	   3. module.exports = ModelController
       4. sekarang kita mulai CRUD-nya
	      - READ data caranya ----->
		    . buat static async readNamastacticasync
			. buat try & catch 
			. isi try 
			  const namavarbebas = await Model.findAll()
			  
			  res.status(200).json({'success read data', namavarbebas'})
			. isi catch 
			  console.log(error)
			  next(error) ----> kalo udah pakai error handler
			  let message = "Internal server error";
              let status = 500; 
			  
			  if (error.name === 'SequelizeDatabaseError') {
                  message = 'Database error';
                  status = 400;
                }
			  res.status(status).json({ message });
			. ---------------------------DAFTAR_CODE---------------------------------
			---------STATUS_CODE--------
			200 OK         ----> Berhasil diproses server (ambil data/read data)
			201 Created    ----> Berhasil membuat (menambahkan data baru)
			204 No Content ----> Berhasil proses tapi kontentnya udah ga ada(misal hapus data tanpa kembalikan data)
			
			--------ERROR_CODE----------
			400 Bad Request           ----> Permintaan client tidak valid/tidak dapat dipahami server.
			401 Unauthorized          ----> autentikasi. Client belum mengirimkan kredensial yang valid.
			                                contoh : akses halaman yang perlu login.
	        403 Forbidden             ----> akses ditolak oleh server.
			                                contoh : staff akses endpoint khusus admin.
            404 Not Found             ----> Sumber tidak ditemukan server.
			                                contoh : akses data yang sudah di hapus.
            500 Internal Server Error ----> Kesalahan dari server(pihak developer).
			                                contoh : konfirgurasi salah, bug dalam kode.

          - CREATE data caranya ----->
		    . buat static async createNamastacticasync/addNamastaticasync
			. buat try & catch 
			. isi try 
			  ----> kalo createnya cuma 1 data 
			        const {namakolom} = req.body
                    const categories = await Category.create({namakolom})

                    res.status(201).json({
                    message: "Success create namatabelnya",
                    namatabelnya
                    })
			  ----> kalo createnya banyak dan perlu login 
			        const { id yang lagi login/userId } = req.loginInfo
                    const { kolom yang di butuhkan apa aja, apa lagi, iya lagi, gapapa lagi } = req.body

                    await Model.create({ isi kolom sama dengan const, UserId: userId *kalo ga sama harus begini* })
 
                    let varbebas = await Model.findOne({
                    where: {
                          kolom *paling beda biar mudah temukan dan anti duplikat*
                    },
                    attributes: {
                    exclude: ["createdAt", "updatedAt"]
                    }
                    })

                    res.status(201).json(varbebas)
			  ----> versi lengkap dengan kondisi login 
			         //console.log(req.loginInfo);
                     const { userId } = req.loginInfo;
                     if (!userId) throw { name: "Unauthorized" };
                     //console.log(userId);
                     const { title, content, imgUrl, categoryId } = req.body;
					 
					 const article = await Article.create({
                                     title,
                                     content,
                                     imgUrl,
                                     categoryId,
                                     authorId: userId,
                                    });
			         res.status(201).json({
                     message: "Success Add Article",
                     article,
                     });
			. isi catch 
			  console.log(error)
			  next(error) ----> kalo udah pakai error handler
			  let message = "Internal server error";
              let status = 500; 
			  
			  if (error.name === 'SequelizeDatabaseError') {
                  message = 'Database error';
                  status = 400;
                }
			  res.status(status).json({ message });
			  
		  - DELETE data caranya ----->
		    . buat static async deleteNamastacticasync
			. buat try & catch 
			. isi try 
			  ----> const { id } = req.params;
                    const varbebas = await Model.findByPk(id);

                    if (!varbebas) throw ({ name: "Article Not Found", id })

                    await Model.destroy({
                        where: {
                                 id,
                               },
                        });

                    res.status(200).json({message: `Success Delete Article with id ${id}`,}); 
			. isi catch 
			  console.log(error)
			  next(error) ----> kalo udah pakai error handler
			  let message = "Internal server error";
              let status = 500; 
			  
			  if (error.name === 'SequelizeDatabaseError') {
                  message = 'Database error';
                  status = 400;
                }
			  res.status(status).json({ message });
			  
			- UPDATE data caranya ----->
		    . buat static async editNamastacticasync/editNamastaticasync
			. buat try & catch 
			. isi try 
			  ----> kalo editnya cuma 1 data 
			        const {id} = req.params
                    const varbebas= await Model.findByPk(id)

                    if(!varbebas) throw ({name : "Model Not Found", id})
    
                    const {kolom} = req.body

                    await Category.update({kolom}, {
                    where : {
                             id
                       }
                    })

                    res.status(200).json({message: `Success edit Model with id ${id}`})
			  ----> kalo editnya banyak dan perlu login 
                    const { id } = req.params
                    const { userId } = req.loginInfo
                    const { nama" kolomnya } = req.body

                    let varbebas = await Model.findByPk(id)

                    if (!varbebas) throw { name: "NotFound" }

                    await Model.update({ nama" kolomnya, UserId: userId }, {
                          where: {
                                   id
                                 }
                    })

                    varbebas = await Model.findOne({
                            where: {
                                    id
                            },
                            attributes: {
                            exclude: ["createdAt", "updatedAt"] ----> ga usah di create lagi
                            }
                    })

                    res.status(201).json(varbebas)
			  ----> versi lengkap dengan kondisi login 
			        const { id } = req.params;

                    const varbebas = await Model.findByPk(id);

                    if (!varbebas) throw { name: "Model Not Found", id };

                    const { title, content, imgUrl, categoryId, authorId } = req.body;
                    await Model.update(
                    { title, content, imgUrl, categoryId, authorId },---> contoh nama kolom lengkap
                    {
                      where: {
                              id,
                             },
                    }
                    );

                    res.status(200).json({message: `Success Update Article with id ${id}`,});
			
			. isi catch 
			  console.log(error)
			  next(error) ----> kalo udah pakai error handler
			  let message = "Internal server error";
              let status = 500; 
			  
			  if (error.name === 'SequelizeDatabaseError') {
                  message = 'Database error';
                  status = 400;
                }
			  res.status(status).json({ message });
			
			- DETAIL data caranya ----->
		    . buat static async detailNamastacticasync
			. buat try & catch 
			. isi try 
			  ---->  const { id } = req.params;
                     const article = await Article.findByPk(id, {
                     include: [User, Category],
                     });
                     // console.log(id)
                     if (!article) throw { name: "Article Not Found", id };

                     res.status(200).json({message: `Success read Article with id ${article.id}`,article,});
			
			. isi catch 
			  console.log(error)
			  next(error) ----> kalo udah pakai error handler
			  let message = "Internal server error";
              let status = 500; 
			  
			  if (error.name === 'SequelizeDatabaseError') {
                  message = 'Database error';
                  status = 400;
                }
			  res.status(status).json({ message });
			  
			  - PATCH data caranya ----->
		    . buat static async detailNamastacticasync
			. buat try & catch 
			. isi try 
			  ---->  const { id } = req.params;
                     const article = await Article.findByPk(id);

                     if (!article) throw { name: "Article Not Found", id };

                     // console.log(req.file)
                     const base64 = req.file.buffer.toString("base64");
                     // console.log(base64);
                     const base64url = `data:image/png;base64,${base64}`
                     // console.log(base64url)
                     let result = await cloudinary.uploader.upload(base64url);

                     const imgUrl = result.url;

                     await Article.update(
                     { imgUrl :result.url },
                      {
                         where: {
                                  id,
                                }
                      }
                     );

                     res.status(200).json({message: `Success Update Article with id ${id}`,});
			
			. isi catch 
			  console.log(error)
			  next(error) ----> kalo udah pakai error handler
			  let message = "Internal server error";
              let status = 500; 
			  
			  if (error.name === 'SequelizeDatabaseError') {
                  message = 'Database error';
                  status = 400;
                }
			  res.status(status).json({ message });
			  
    -----------------------------------------------PAGINATION,SORTING,SEARCH---------------------------------------------------------------
   
2. buat akses endpoint dengan copas getting started dari express.
   - app.use(express.urlencoded({ extended: true })) -----> biar bisa baca data dari body
   - app.use(express.json()) -----> biar bisa baca data json
   
3. buat hakakses dan batasan akses.
   - di folder middleware buat auth.js 
   - const verifyToken dari JWT
   - const model
   - buat 2 fungsi untuk authorization dan authentication
     >>>> const authentication = async (req, res, next) => {
          try {
          const { authorization } = req.headers

          if (!authorization) throw { name: "Unauthorized" }

          const access_token = authorization.split(' ')[1]

          const payload = verifyToken(access_token)

          if (!user) throw { name: "Unauthorized" }

          req.loginInfo = {
            userId: user.id,
            email: user.email
          }

          next()
         } catch (error) {
          next(error)
         }
        }
	>>>> const authorization = async (req, res, next) => {
         try {
         const { id } = req.params
         const { userId } = req.loginInfo

         const user = await User.findByPk(userId)

         if (!user) throw { name: "Forbidden" }  

         const grocery = await Grocery.findByPk(id)

         if (!grocery) throw { name: "NotFound" }

         if (grocery.UserId !== user.id) throw { name: "Forbidden" }

         next()
         } catch (error) {
         next(error)
         }}


4. pasang di app.js 
5. pasang eror handler
6. insert postman collection.
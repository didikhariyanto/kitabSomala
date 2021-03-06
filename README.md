# 1.spring-boot
	-Jpa<depedency>
	-MySql<depedency>

# 2.buat folder entity
	- buat class sesuai kebutuhan
	- jika sudah tambah getter and setter

# 3.Buat folder dao<data acces object>
	- buat sesuai kebutuhan class entity
	- tambahkan [extends PagingAndSortingRepository<Anggota, String>]

# 4. buat folder controller
	- buat class sesuai kebutuhan

# 5. koneksi database
	- buka --> src/main/resources/aplication.properties
	- tambahkan seperti ini:
		 //  koneksi mysql
		 spring.datasource.url=jdbc:mysql://localhost/aplikasicrud
		 spring.datasource.username=aplikasicruduser
		 spring.datasource.password=aplikasicrudpasswd

		// membuat generate database dari maping entity
		spring.jpa.generate-ddl=true
		spring.jpa.show-sql=true
		spring.jpa.properties.hibernate.format_sql=true

		// merapikan data api json
		spring.jackson.serialization.indent_output=true

		// supaya tidak re run spring boot
		spring.thymeleaf.cache=false

# 6.crud json browser
***
tambahkan depedency starter web

    <dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>


  ***
	// jika method GET menjalankan ini.
	@RequestMapping(value = "/anggota", method = RequestMethod.GET)
	public Page<Anggota> cariAnggota(Pageable page) {
		return ad.findAll(page);
	}

***
// jika insert data menjalankan ini
	@RequestMapping(value = "/anggota", method = RequestMethod.POST)
	@ResponseStatus(HttpStatus.CREATED)
	public void insertAnggotaBaru(@RequestBody @Valid Anggota p) {
		ad.save(p);
	}

***
	// jika update data menjalankan ini
		@RequestMapping(value = "/anggota/{id}", method = RequestMethod.PUT)
		@ResponseStatus(HttpStatus.OK)
		public void updateAnggota(@PathVariable("id") String id, @RequestBody @Valid Anggota p) {
			p.setId(id);
			ad.save(p);
		}

***
		// mencari data dan respon 404 jika data tidak ada <cari berdasarkan id>
		@RequestMapping(value = "/anggota/{id}", method = RequestMethod.GET)
		@ResponseStatus(HttpStatus.OK)
		public ResponseEntity<Anggota> cariAnggotaById(@PathVariable("id") String id) {
			Anggota hasil = ad.findOne(id);
			if (hasil == null) {
				return new ResponseEntity<>(HttpStatus.NOT_FOUND);
			}
			return new ResponseEntity<>(hasil, HttpStatus.OK);
		}

***
		// jika delete data menjalankan ini
		@RequestMapping(value = "/anggota/{id}", method = RequestMethod.DELETE)
		@ResponseStatus(HttpStatus.OK)
		public void hapusAnggota(@PathVariable("id") String id) {
			ad.delete(id);
		}


# 7. deklarasi aturan java validasi pada entity java class.

***
			@NotNull
			@NotEmpty (tidak boleh di gunakan pada tipe data Date, karena bukan String)
			@Size(min=3, max=150)
			@Past (masa lampau, di gunakan untuk data tipe Date)
			@Email

***
			- tambahin parameter @valid pada nama controller class java. misal insert dan update
			contoh coding;

				-------------------------------------------------------------------
					// jika insert data menjalankan ini
			@RequestMapping(value = "/anggota", method = RequestMethod.POST)
			@ResponseStatus(HttpStatus.CREATED)
			public void insertAnggotaBaru(@RequestBody @Valid Anggota p) {
				ad.save(p);
			}
			---------------------------------------------------------------------


# 8. Themplate engine with thymeleaf


tambahkan depedency sebaigai berikut

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-thymeleaf</artifactId>
		</dependency>
***
    tambahkan folder"Templates" pada

    src/main/resources/templates


  - Anotation @controller
    tidak usah @RestCntroller supaya tidak respon body
  - tambahkan @ResponBody di atas method


# 9 CSRF spring-boot

***
tambahkan PADA HTML DI BAWAH forM

<input type="hidden" name="${_csrf.parameterName}" value="${_csrf.token}"/>

# Manajemen User

Lanjutan Pratikum kali ini kita akan membuat program yang dapat mengatur user dalam sistem, dalam hal ini yang dapat melakukan pengaturan hanya admin saja, akan tetapi karena kita belum membahas program login maka belum dapat mengatur hak akses untuk masing-masing user, maka  hasil dari program kita sementara ini masih dapat dibuka siapa saja. 
![Image]( hasil.PNG )
## Langkah 1: Rancang Tabel
Buat tabel pada database dengan nama `user` dengan struktur seperti dibawah ini:<br>
![Image]( tabel.PNG )

## Langkah 2: Rancang Tampilan (UI)
Untuk membuat program kita tersusun dengan rapi maka semua file program yang terdapat pada administrator kita simpan dalam folder yang terpisah, program ini akan disimpan dalam folder `user` yang diletakan dalam folder `view` sehingga `view\user`. Tampilan program merupakan modifikasi dari program SB2Admin yang sudah pernah kita pergunakan sebelunya. Terdapat 5 File dalam folder`user` ini: 
1. header.php
2. footer.php
3. user.php
4. tambah.php
5. edit.php

### header.php
```php
<!DOCTYPE html>
<html lang="en">

<head>

    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <meta name="description" content="">
    <meta name="author" content="">

    <title>Teknik Antrian</title>

    <!-- Custom fonts for this template-->
    <link href="<?php echo base_url('asset/'); ?>sb2_admin/vendor/fontawesome-free/css/all.min.css" rel="stylesheet" type="text/css">
    <link href="https://fonts.googleapis.com/css?family=Nunito:200,200i,300,300i,400,400i,600,600i,700,700i,800,800i,900,900i" rel="stylesheet">
    <!-- Custom styles for this template-->
    <link href="<?php echo base_url('asset/'); ?>sb2_admin/css/sb-admin-2.min.css" rel="stylesheet">
    <link href="<?php echo base_url('asset/'); ?>sb2_admin/vendor/datatables/dataTables.bootstrap4.min.css" rel="stylesheet">

</head>

<body id="page-top">
    <div id="wrapper">
        <ul class="navbar-nav bg-gradient-dark sidebar sidebar-dark" id="accordionSidebar">
            <a class="sidebar-brand d-flex align-items-center justify-content-center" href="index.html">
                <div class="sidebar-brand-icon">
                    <i class="fas fa-fw fa-user"></i>
                </div>
                <div class="sidebar-brand-text mx-3">Admin</div>
            </a>
            <hr class="sidebar-divider">
            <div class="sidebar-heading">
                Menu
            </div>
            <li class="nav-item <?php if ($page == 'user') {
                                    echo 'active';
                                }
                                ?>">
                <a class="nav-link" href="<?php echo base_url() ?>user">
                    <i class="fas fa-fw fa-folder"></i>
                    <span>Manajemen User </span></a>
            </li>
            <hr class="sidebar-divider">
            <div class="text-center d-none d-md-inline">
                <button class="rounded-circle border-0" id="sidebarToggle"></button>
            </div>
        </ul>
        <div id="content-wrapper" class="d-flex flex-column">
            <div id="content">
                <nav class="navbar navbar-expand navbar-light bg-gradient-info topbar mb-4 static-top shadow">
                    <button id="sidebarToggleTop" class="btn btn-link d-md-none rounded-circle mr-3">
                        <i class="fa fa-bars text-white"></i>
                    </button>
                    <ul class="navbar-nav ml-auto">
                        <div class="topbar-divider d-none d-sm-block"></div>
                        <li class="nav-item dropdown no-arrow">
                            <a class="nav-link dropdown-toggle" href="#" id="userDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                                <span class="mr-2 d-none d-lg-inline text-white small">
                                    User
                                </span> </a>
                            <div class="dropdown-menu dropdown-menu-right shadow animated--grow-in" aria-labelledby="userDropdown">
                                <a class="dropdown-item" href="<?php echo base_url() ?>user/logout">
                                    <i class="fas fa-sign-out-alt fa-sm fa-fw mr-2 text-gray-400"></i>
                                    Logout
                                </a>
                            </div>
                        </li>

                    </ul>
                </nav>
                <!-- End of Topbar -->
```
dari program header.php diatas perhatikan program dibawah ini:
```php
<li class="nav-item <?php if ($page == 'user') {
                        echo 'active';
                    }
                    ?>">
    <a class="nav-link" href="<?php echo base_url() ?>user">
        <i class="fas fa-fw fa-folder"></i>
        <span>Manajemen User </span></a>
</li>
```
`$page` adalah variabel yang dikirim dari controller untuk menentukan halaman terpilih sehingga menu dapat diberikan efek `active`

### footer.php
```php
</div>
<footer class="sticky-footer bg-gradient-info">
    <div class="container my-auto">
        <div class="copyright text-center my-auto text-white">
            <span>Copyright &copy; Bank Jago syariah</span>
        </div>
    </div>
</footer>
</div>
</div>

<!-- Scroll to Top Button-->
<a class="scroll-to-top rounded" href="#page-top">
    <i class="fas fa-angle-up"></i>
</a>

<!-- Bootstrap core JavaScript-->
<script src="<?php echo base_url('asset/'); ?>sb2_admin/vendor/jquery/jquery.min.js"></script>
<script src="<?php echo base_url('asset/'); ?>sb2_admin/vendor/bootstrap/js/bootstrap.bundle.min.js"></script>

<!-- Core plugin JavaScript-->
<script src="<?php echo base_url('asset/'); ?>sb2_admin/vendor/jquery-easing/jquery.easing.min.js"></script>

<!-- Custom scripts for all pages-->
<script src="<?php echo base_url('asset/'); ?>sb2_admin/js/sb-admin-2.min.js"></script>

<!-- Page level plugins -->
<script src="<?php echo base_url('asset/'); ?>sb2_admin/vendor/datatables/jquery.dataTables.min.js"></script>
<script src="<?php echo base_url('asset/'); ?>sb2_admin/vendor/datatables/dataTables.bootstrap4.min.js"></script>

<!-- Page level custom scripts -->
<script src="<?php echo base_url('asset/'); ?>sb2_admin/js/demo/datatables-demo.js"></script>
</body>
</html>
```
### user.php
```php
<div class="container-fluid">
    <!-- Page Heading -->
    <div class="d-sm-flex align-items-center justify-content-between mb-4">
        <h3 class="h3 mb-0 text-gray-800 d-none d-sm-inline-block ">Manajemen User</h3>
        <a href="<?php echo base_url('') ?>user/tambah_user/" class="btn btn-sm btn-primary shadow-sm"><i class="fas fa-download fa-sm text-white-50"></i> Tambah Data User</a>
    </div>
    <div class="card shadow mb-4">
        <div class="card-body">
            <div class="table-responsive">
                <table class="table table-bordered" id="dataTable" width="100%" cellspacing="0">
                    <thead>
                        <tr>
                            <th>NO</th>
                            <th>Nama User</th>
                            <th>Level</th>
                            <th>User</th>
                            <th>Aktif</th>
                            <th>Aksi</th>
                        </tr>
                    </thead>
                    <tbody>
                        <?php $no = 0;
                        foreach ($user as $usr) {
                            $no++ ?>
                            <tr>
                                <td><?php echo $no; ?></td>
                                <td><?php echo $usr['nama'] ?></td>
                                <td><?php if ($usr['level'] == 0) {
                                        echo "Belum ditentukan";
                                    } elseif ($usr['level'] == 1) {
                                        echo "Admin";
                                    } elseif ($usr['level'] == 2) {
                                        echo "Teller/CS";
                                    }
                                    ?></td>
                                <td><?php echo $usr['user'] ?></td>
                                <td>
                                    <?php if ($usr['aktif'] == 1) { ?>
                                        <a href="<?php echo base_url(); ?>user/aktif/<?php echo $usr['id_login'] ?>/<?php echo $usr['aktif'] ?>" class="btn btn-success btn-icon-split btn-sm">
                                            <span class="icon text-white-50">
                                                <i class="fas fa-check"></i>
                                            </span>
                                            <span class="text">Aktif</span>
                                        <?php } else { ?>
                                        </a>
                                        <a href="<?php echo base_url(); ?>user/aktif/<?php echo $usr['id_login'] ?>/<?php echo $usr['aktif'] ?>" class="btn btn-danger btn-icon-split btn-sm">
                                            <span class="icon text-white-50">
                                                <i class="fas fa-exclamation-triangle"></i>
                                            </span>
                                            <span class="text">Nonaktif</span>
                                        </a>
                                    <?php } ?>
                                </td>
                                <td align="center">
                                    <a href="<?php echo base_url(); ?>user/edit/<?php echo $usr['id_login'] ?>" class="btn btn-primary btn-circle btn-sm" title="Edit">
                                        <i class="fas fa-edit"></i>
                                    </a>
                                    <a href="<?php echo base_url(); ?>user/hapus/<?php echo $usr['id_login'] ?>" class="btn btn-danger btn-circle btn-sm" title="Hapus">
                                        <i class="fas fa-trash"></i>
                                    </a>
                                </td>
                            </tr>
                        <?php } ?>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</div>
```
Program user akan menampilkan isi tabel user dari database, menggunakan tabel html yang sudah ditambahkan `jquery` untuk memanipulasi  isi tabel tersebut. Saat ini tabel user masih kosong untuk itu kita akan melakukan penembah data melalui program berikutnya
### tambah.php
```php
<?php echo form_open_multipart('user/tambah'); ?>
<div class="container-fluid">
    <div class="row">
        <div class="col-lg-12">
            <div class="card shadow mb-4">
                <div class="card-header py-3">
                    <h6 class="m-0 font-weight-bold text-primary">Tambah User</h6>
                </div>
                <div class="card-body">
                    <div class="row">
                        <div class="col-lg-7 mx-auto">
                            <div class="row g-3">
                                <div class="col-12 mb-3">
                                    <label class="form-label">Nama Pegawai<small class="text-danger">*</small> </label>
                                    <input name="tnama" type="text" class="form-control" value="<?php echo set_value('tnama'); ?>">
                                    <?php echo form_error('tnama', '<small class="text-danger">', '</small>');
                                    ?>
                                </div>
                                <div class=" col-12 mb-3">
                                    <label for="alamat_calon" class="form-label">Jenis User</label>
                                    <select class="form-control form-select-sm" name="tlevel">
                                        <option value="0">Pilih Jenis User</option>
                                        <option value="1">Admin </option>
                                        <option value="2">Teller/CS</option>
                                    </select>
                                </div>
                                <div class="col-12 mb-3">
                                    <label class="form-label">User<small class="text-danger">*</small> </label>
                                    <input name="tuser" type="text" class="form-control" value="<?php echo set_value('tuser'); ?>">
                                    <?php echo form_error('tuser', '<small class="text-danger">', '</small>');
                                    ?>
                                </div>
                                <div class="col-12 mb-3">
                                    <label class="form-label">Password<small class="text-danger">*</small> </label>
                                    <input name="tpass" type="text" class="form-control" value="<?php echo set_value('tpass'); ?>">
                                    <?php echo form_error('tpass', '<small class="text-danger">', '</small>');
                                    ?>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <div class="d-sm-flex align-items-center justify-content-between mb-4">
        <h1 class="h3 mb-0 text-gray-800"></h1>
        <button type="submit" class="btn btn-sm btn-primary shadow-sm"><i class="fas fa-download fa-sm text-white-50"></i> Tambah Pengumuman</button>
    </div>
</div>
</form>
```
`<?php echo form_open_multipart('user/tambah'); ?>` yang digunakan dalam program diatas merupakan fasilitas yang diberikan CodeIgniter untuk memudahkan menjalankan fungsi-fungsi form.
### edit.php
```php
<?php echo form_open_multipart('user/editp'); ?>
<div class="container-fluid">
    <div class="row">
        <div class="col-lg-12">
            <div class="card shadow mb-4">
                <div class="card-header py-3">
                    <h6 class="m-0 font-weight-bold text-primary">Edit User</h6>
                </div>
                <div class="card-body">
                    <div class="row">
                        <div class="col-lg-7 mx-auto">
                            <div class="row g-3">
                                <div class="col-12 mb-3">
                                    <input name="tid" type="text" class="form-control" value="<?php echo $user['id_login']
                                                                                                ?>" hidden>
                                    <label class="form-label">Nama Pegawai<small class="text-danger">*</small> </label>
                                    <input name="tnama" type="text" class="form-control" value="<?php echo $user['nama']; ?>">
                                    <?php echo form_error('tnama', '<small class="text-danger">', '</small>');
                                    ?>
                                </div>
                                <div class=" col-12 mb-3">
                                    <label for="alamat_calon" class="form-label">Jenis User</label>
                                    <select class="form-control form-select-sm" name="tlevel">
                                        <option value="0" <?php if ($user['level'] == 0) {
                                                                echo "selected";
                                                            } ?>>Pilih Jenis User</option>
                                        <option value="1" <?php if ($user['level'] == 1) {
                                                                echo "selected";
                                                            } ?>>Admin </option>
                                        <option value="2" <?php if ($user['level'] == 2) {
                                                                echo "selected";
                                                            } ?>>Teller/CS</option>
                                    </select>
                                </div>
                                <div class="col-12 mb-3">
                                    <label class="form-label">User Baru </label>
                                    <input name="tuser" type="text" class="form-control" placeholder="Isi user bila ingin merubah">
                                    <?php echo form_error('tuser', '<small class="text-danger">', '</small>');
                                    ?>
                                </div>
                                <div class="col-12 mb-3">
                                    <label class="form-label">Password Baru </label>
                                    <input name="tpass" type="text" class="form-control" placeholder="Isi Password bila ingin merubah">
                                    <?php echo form_error('tpass', '<small class="text-danger">', '</small>');
                                    ?>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <div class="d-sm-flex align-items-center justify-content-between mb-4">
        <h1 class="h3 mb-0 text-gray-800"></h1>
        <button type="submit" class="btn btn-sm btn-primary shadow-sm"><i class="fas fa-download fa-sm text-white-50"></i>Simpan Perubahan</button>
    </div>
</div>
</form>
```
## Langkah 3: Controler User.php
Selanjutnya kita ciptakan file `User.php` dalam folder controller. Ingat karena ini merupakan controller maka wajib diawali dengan huruf kapital. didalam controller ini ada terdapat 7 function utama selain constructor.
1. `function index` untuk menampilkan tampilan awal yang menjalakan view user.php
2. `function tambah_user` untuk menjalakan view tambah.php
3. `function tambah` untuk menjalankan proses tambah data dan memverifikasi jika ada kesalahan data
4. `function aktif` untuk merubah user menjadi aktif atau non aktif 
5. `function hapus` untuk menhapus user
6. `function edit` untuk menjalankam view edit.php
7. `function editp` untuk melakukan proses edit.

```php
<?php
defined('BASEPATH') or exit('No direct script access allowed');

class User extends CI_Controller
{
    public function __construct()
    {
        parent::__construct();

        $this->load->library('form_validation');
    }
    public function index()
    {
        $data['page'] = "user";
        $data['user'] = $this->db->get('user')->result_array();
        $this->load->view('user/header', $data);
        $this->load->view('user/user');
        $this->load->view('user/footer');
    }
    public function tambah_user()
    {
        $this->load->view('user/header');
        $this->load->view('user/tambah');
        $this->load->view('user/footer');
    }
    public function tambah()
    {
        //validasi kegiatan form
        $this->form_validation->set_rules("tnama", "Nama Pegawai", "required|trim", [
            'required' => 'Nama Pegawai tidak boleh kosong'
        ]);
        $this->form_validation->set_rules("tuser", "User", "required|trim|min_length[5]|is_unique[user.user]", [
            'required' => 'User tidak boleh kosong',
            'is_unique' => 'User sudah terdaftar',
            'min_length' => 'User minimal 5 karekter'
        ]);
        $this->form_validation->set_rules("tpass", "Password", "required|trim|min_length[5]", [
            'required' => 'Password tidak boleh kosong',
            'is_unique' => 'password sudah terdaftar',
            'min_length' => 'Password minimal 5 karekter'
        ]);

        //Uji validasi form diatas apabila belum memenuhi prosedur   
        if ($this->form_validation->run() == false) {
            $this->load->view("user/header");
            $this->load->view("user/tambah");
            $this->load->view("user/footer");
        } else {
            $data = [
                'nama' => htmlspecialchars($this->input->post('tnama', true)),
                'level' => htmlspecialchars($this->input->post('tlevel', true)),
                'user' => htmlspecialchars($this->input->post('tuser', true)),
                'pass' => password_hash($this->input->post('tpass', true), PASSWORD_DEFAULT),
                'aktif' => 0
            ];
            $this->db->insert('user', $data);
            $this->session->set_flashdata('pesan', '  <div class="container alert alert-dismissible alert-success mt-3 mb-5">
            <p class="mb-0">User berhasil ditambahkan</p>
        </div>');
            redirect('user');
        }
    }

    public function aktif($id = "", $aktif = "")
    {
        if ($aktif == 0) {
            $this->db->where('id_login', $id);
            $this->db->update('user', ['aktif' => 1]);
        } else {
            $this->db->where('id_login', $id);
            $this->db->update('user', ['aktif' => 0]);
        }

        redirect('user');
    }
    public function hapus($id = "")
    {
        $this->db->delete('user', ['id_login' => $id]);
        redirect('user');
    }
    public function edit($id = "")
    {

        $data['user'] = $this->db->get_where('user', ['id_login' => $id])->row_array();
        $this->load->view('user/header', $data);
        $this->load->view('user/edit');
        $this->load->view('user/footer');
    }
    public function editp()
    {
        if ($this->input->POST('tuser') != "") {
            $data = ['user' => $this->input->POST('tuser', true)];
            $this->db->where('id_login', $this->input->POST('tid'));
            $this->db->update('user', $data);
        }
        if ($this->input->POST('tpass') != "") {
            $data = ['user' => $this->input->POST('tpass', true)];
            $this->db->where('id_login', $this->input->POST('tid'));
            $this->db->update('user', $data);
        }
        $data = [
            'nama' => $this->input->POST('tnama', true),
            'level' => $this->input->POST('tlevel')
        ];
        $this->db->where('id_login', $this->input->POST('tid'));
        $this->db->update('user', $data);
        redirect('user');
    }
}
```
## Langkah 4: Aktifkan Session
dalam program kita ini terdapat perintah yang menggunakan sesion pada saat mengirimkan pesan kesalahan `  $this->session->set_flashdata` seperti pada saat berhasil menambahkan data dan apabila data yang diisi kosong, dan terdapat juga program `$this->form_validation->run()` untuk mengirimkan validasi apakah penyimpanan dilakukan pengguna atau tidak. perintah-perintah tersebut ada pada `libraries` yang terdapat pada file `autolod.php` untuk itu silahkan edit file tersebut pada bagian berikut:
```php
$autoload['libraries'] = array('database', 'session');
```
kalau semua sudah dikerjakan silahkan jalankan program dengan memanggil alamat `http://localhost/teknik_antrian/user` sesuaikan dengan base url masing-masing, jika terdapat kesalah periksa kembali satu persatu kodingan dan bisa bertanya melalui classroom

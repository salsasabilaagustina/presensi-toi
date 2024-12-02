<template>
    <div class="row">
        <div class="col">
            <h1 class="text-center my-3 judul"> LAPORAN RIWAYAT KEHADIRAN </h1> 
        </div>
     </div>
     <div class="row  justify-content-center">
        <input type="date" @change="getSchedule" style="width: 110rem; height:3rem;background-color: #D9D9D9;" class="rounded-3 my-3" >
        <h4>Tanggal : {{ date }}</h4>
    </div>
     <div class="row justify-content-center">
            <div class="col-11">
             <table class="table table-striped table-bordered">
                <thead>
                    <tr>
                        <th class="text-center" rowspan ="2">No</th>
                        <th class="text-center" rowspan="2">Tingkat</th>
                        <th class="text-center" rowspan="2">Jumlah Siswa</th>
                        <th class="text-center" colspan="5">KETERANGAN</th>
                    </tr>
                    <tr>
                        <th>H</th>
                        <th>I</th>
                        <th>S</th>
                        <th>A</th>
                        <th>D</th>
                    </tr>
                    <tr v-for="(item, index) in presensi" :key="index">
                        <td>{{ index + 1 }}</td>
                        <td>{{ item.kelas }}</td>
                        <td>{{ item.siswa.length }}</td>
                        <td>{{ countKehadiran(item.siswa, 'H') }}</td>
                        <td>{{ countKehadiran(item.siswa, 'I') }}</td>
                        <td>{{ countKehadiran(item.siswa, 'S') }}</td>
                        <td>{{ countKehadiran(item.siswa, 'A') }}</td>
                        <td>{{ countKehadiran(item.siswa, 'D') }}</td>
                    </tr>
                </thead>
             </table>
             <div class="col-1 ms-auto mb-3">
                <NuxtLink to="/">
                    <button type="button" class="btn" style="background-color: #D9D9D9;">Kembali</button>
                </NuxtLink>
             </div>
         </div>
         </div>
</template>

<script setup>
const supabase = useSupabaseClient()
const date = ref('')
const presensi = ref([])

// Fungsi untuk mengambil data laporan berdasarkan tanggal yang dipilih
const getSchedule = async (event) => {
    date.value = event.target.value;

    const { data, error } = await supabase
        .from('kategori_kelas')
        .select(`
            kelas,
            siswa (
                *,
                daftar_kehadiran ( * )
            )
        `)
        .eq('siswa.daftar_kehadiran.tgl', date.value);  // Filter berdasarkan tanggal yang dipilih

    if (error) {
        console.error('Error fetching presensi:', error);
    } else {
        presensi.value = data
        // Lakukan pengolahan data (count) di frontend
        // const reportData = {};

        // // Proses data untuk menghitung H, I, S, A
        // data.forEach(item => {
        //     const kelas = item.kategori_kelas.nama;
        //     if (!reportData[kelas]) {
        //         reportData[kelas] = { hadir: 0, sakit: 0, ijin: 0, alfa: 0, jumlah_siswa: 0 };
        //     }

        //     reportData[kelas].jumlah_siswa++;

        //     switch (item.keterangan) {
        //         case 'H': reportData[kelas].hadir++; break;
        //         case 'S': reportData[kelas].sakit++; break;
        //         case 'I': reportData[kelas].ijin++; break;
        //         case 'A': reportData[kelas].alfa++; break;
        //         default: break;
        //     }
        // });

        // // Convert object ke array untuk ditampilkan
        // presensi.value = Object.entries(reportData).map(([kelas, counts]) => ({
        //     tingkat: kelas,
        //     ...counts
        // }));
    }
};

const countKehadiran = (students, keterangan) => {
    return students.filter(student => student.daftar_kehadiran[0]?.keterangan === keterangan).length
}

// Menjalankan getSchedule saat komponen pertama kali dimuat
onMounted(() => {
    // Dapatkan data untuk tanggal default (hari ini misalnya)
    getSchedule({ target: { value: new Date().toISOString().split('T')[0] } });
});
</script>
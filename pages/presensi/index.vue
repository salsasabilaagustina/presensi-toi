<template>
    <div class="row">
        <div class="col">
            <h1 class="text-center my-3">DAFTAR KEHADIRAN </h1>
        </div>
    </div>
    <div class="row justify-content-center">
        <div class="col-11">
            <select @change="getSiswa" class="form-control form-control-lg form-select mb-3" style="background-color: #D9D9D9;">
                <option value="" disabled>Pilih Tingkat</option>
                <option v-for="k in kelas" :key="k.id" :value="k.id">{{ k.kelas }}</option>
            </select>

        </div>
    </div>
    <div class="row justify-content-center">
        <div class="col-11">
            <!-- {{ siswaMarked }} -->
            <!-- <div class="my-3 h3">Jumlah Siswa: {{ siswaMarked.length }}</div> -->
            <table class="table table-striped table-bordered">
                <thead>
                    <tr>
                        <th rowspan="2">Nama Siswa</th>
                        <th class="text-center" colspan="5">KETERANGAN</th>
                    </tr>
                    <tr>
                        <th>H</th>
                        <th>I</th>
                        <th>S</th>
                        <th>A</th>
                        <th>D</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(s, i) in siswa" :key="i">
                        <td>{{ s.nama }}</td>
                        <td><input v-model="siswaMarked[i].keterangan" type="radio" value="H" checked></td>
                        <td><input v-model="siswaMarked[i].keterangan" type="radio" value="I"></td>
                        <td><input v-model="siswaMarked[i].keterangan" type="radio" value="S"></td>
                        <td><input v-model="siswaMarked[i].keterangan" type="radio" value="A"></td>
                        <td><input v-model="siswaMarked[i].keterangan" type="radio" value="D"></td>
                    </tr>
                </tbody>
            </table>
            <div class="position-relative my-5">
                <NuxtLink to="/">
                    <button class="btn position-absolute bottom-0 start-0 " style="background-color: #D9D9D9">kembali</button>
                </NuxtLink>
                     <button @click="kirimPresensi" class="btn position-absolute bottom-0 end-0" style="background-color: #D9D9D9">Kirim</button>
            </div>
            <div class="row">
                <NuxtLink to="/logout">
                    Logout
                </NuxtLink>
            </div>
        </div>
    </div>
</template> 

 
<script setup>
definePageMeta({
  middleware: 'auth',
})

const client = useSupabaseClient()
const kelas = ref([])
const siswa = ref([])
const siswaMarked = ref([{}])

async function getKelas() {
    const { data, error } = await client
        .from('kategori_kelas')
        .select()
    if(data) {
        kelas.value = data
    }
}

async function getSiswa(event) {
    siswaMarked.value = []
    let id_kelas = event.target.value
    const { data, error } = await client
        .from('siswa')
        .select()
        .eq('kelas', id_kelas)
    if(data) {
        siswa.value = data
        for (let i = 0; i < data.length; i++) {
            siswaMarked.value.push({
                id_siswa: data[i].id, 
                keterangan: "H"
            })
        }
    }
}

async function kirimPresensi() {
    // console.log(siswaMarked.value)
    const { data, error } = await client
        .from('daftar_kehadiran')
        .insert(siswaMarked.value)
        .select()
    if(data) {
        navigateTo('/presensi/riwayat')
    }

}


onMounted(() => {
    getKelas()
})
</script>
<template>
    <div class="row">
        <div class="col">
            <h1 class="text-center my-4">RIWAYAT KEHADIRAN </h1>
        </div>
    </div>
    <div class="row justify-content-center align-items-center">
        <div class="col-auto">
            <select v-model="selectedKelas" @change="getSiswa" class="form-control rounded-3 my-3"
                style=" height:3rem; background-color: #D9D9D9;">
                <option disabled :value="null">Pilih Tingkat</option>
                <option v-for="k in kelas" :key="k.id" :value="k.id">{{ k.kelas }}</option>
            </select>
        </div>
        <div class="col-auto">
            <input type="date" @change="schedulePicked" style=" height:3rem;background-color: #D9D9D9;"
                class="form-control rounded-3 my-3">
        </div>
    </div>
    <div class="row justify-content-center">
        <div class="col-11">
            <table class="table table-striped table-bordered">
                <thead>
                    <tr>
                        <th class="text-center">No</th>
                        <th class="text-center">Nama</th>
                        <th class="text-center">Kelas</th>
                        <th class="text-center">Tanggal</th>
                        <th class="text-center">Keterangan</th>
                    </tr>
                    <tr v-for="(student, i) in students" :key="i">
                        <td>{{ i + 1 }}.</td>
                        <td>{{ student.siswa?.nama }}</td>
                        <td>{{ student.siswa?.kategori_kelas?.kelas }}</td>
                        <td>{{ student.tgl }}</td>
                        <td>{{ student.keterangan }}</td>
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
const kelas = ref([])
const students = ref([])

const selectedKelas = ref(null)

async function getKelas() {
    const { data, error } = await supabase
        .from('kategori_kelas')
        .select()
    if (data) {
        kelas.value = data
    }
}

async function schedulePicked(event) {
    date.value = event.target.value
    getSiswa()
}

async function getSiswa() {
    let query = supabase
        .from('daftar_kehadiran')
        .select(`
        *,
        siswa!inner (
            nama,
            kategori_kelas (
                kelas
            )
        )
    `)
    if (selectedKelas.value) query = query.eq('siswa.kelas', selectedKelas.value)
    if (date.value) query = query.eq('tgl', date.value)
    const { data, error } = await query
    if (error) throw error
    if (data) students.value = data
}

onMounted(() => {
    getKelas()
})
</script>
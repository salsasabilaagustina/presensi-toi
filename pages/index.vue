<template>
    <div class="container-fluid">
       <div class="row justify-content-around align-items-center" style="height: 80vh;">
                
           <div class="col-sm-3">
           <NuxtLink to="presensi/riwayat" class="bg-primary">
               <div class="card rounded-3 bg4 shadow">
                   <h2 class="card-title">Riwayat</h2>
                 
               </div>
           </NuxtLink>  
           </div>
            
           <div class="col-sm-3">
               <NuxtLink to="presensi/">
                  <div class="card rounded-3 bg5 shadow">
                      <h2 class="card-title">Presensi</h2>
                     
                   </div>
           </NuxtLink>
           </div>

           <div class="row justify-content-center">
           <div class="col-sm-3 my-3">
               <NuxtLink to="laporan/">
                  <div class="card rounded-3 bg6 shadow">
                      <h2 class="card-title">Laporan absensi</h2> 
                   </div>
           </NuxtLink>
           </div>
           </div>
   
       </div>
    </div>   
   </template>
   
   <style scoped>
   .card{
       height: 10rem;
   }
   a{
       text-decoration: none;
       color: #2E4D71;
   }
   .card {
       position: relative;
   }
   .card .card-title {
       position: relative;
       top: 10;
       left: 10px;
       font-size: 40px;
   }
   
   .bg4 {
       background-image: url("/assets/img/bg4.jpeg");
       background-size: cover;
       background-repeat: no-repeat;
       background-position: 15%;
   }
   .bg5 {
       background-image: url("/assets/img/bg5.jpeg");
       background-size: cover;
       background-repeat: no-repeat;
       background-position: 2%;
   }
   .bg6 {
       background-image: url("/assets/img/bg6.png");
       background-size: cover;
       background-repeat: no-repeat;
       background-position: 2%;
   }
   </style>
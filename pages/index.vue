<script setup lang="ts">
/**
 *
 */
documentTitle('Proyecto')
definePageMeta({
  layout: 'new-layout',
})

const datos = ref({
  ranking: 819,
  nem: 819,
  lectora: 700,
  mate_1: 800,
  mate_2: 800,
  ciencias: 750,
  historia: 0,
  universidad: '',
})

const universidades = ref([
  {
    carrera: 'Química y Farmacia',
    universidad: 'U. de Chile',
    ultimo_seleccionado: 774.8,
    ultimo_matriculado: 773.8,
    ponderaciones: {
      ranking: 20,
      nem: 10,
      lectora: 10,
      mate_1: 20,
      mate_2: 10,
      ciencias: 30,
      historia: 0,
    },
  },

  {
    carrera: 'Química y Farmacia',
    universidad: 'Universidad San Sebastian',
    ultimo_seleccionado: 0,
    ultimo_matriculado: 0,
    ponderaciones: {
      ranking: 10,
      nem: 10,
      lectora: 20,
      mate_1: 50,
      mate_2: 0,
      ciencias: 10,
      historia: 0,
    },
  },
  {
    carrera: 'Química y Farmacia',
    universidad: 'U. Católica',
    ultimo_seleccionado: 0,
    ultimo_matriculado: 882.25,
    ponderaciones: {
      ranking: 20,
      nem: 20,
      lectora: 15,
      mate_1: 20,
      mate_2: 0,
      ciencias: 25,
      historia: 0,
    },
  },

  {
    carrera: 'Ingeniería Civil en Computación',
    universidad: 'U. de Chile',
    ultimo_seleccionado: 0,
    ultimo_matriculado: 828.6,
    ponderaciones: {
      ranking: 25,
      nem: 10,
      lectora: 10,
      mate_1: 20,
      mate_2: 20,
      ciencias: 15,
      historia: 0,
    },
  },
])

/**
 *
 */
const selectedUniversidad = computed(() => {
  if (datos.value.universidad.length > 0) {
    return universidades.value[parseInt(datos.value.universidad)]
  }
  return false
})

type Ponderaciones = {
  ranking: number
  nem: number
  lectora: number
  mate_1: number
  mate_2: number
  ciencias: number
  historia: number
}
/**
 *
 */
const calcularResultado = (ponderaciones: Ponderaciones) => {
  const { ranking, nem, lectora, mate_1, mate_2, ciencias, historia } =
    datos.value

  const rankingTotal = (ranking * ponderaciones.ranking) / 100
  const nemTotal = (nem * ponderaciones.nem) / 100
  const lectoraTotal = (lectora * ponderaciones.lectora) / 100
  const mate1 = (mate_1 * ponderaciones.mate_1) / 100
  const mate2 = (mate_2 * ponderaciones.mate_2) / 100
  const cienciasTotal = (ciencias * ponderaciones.ciencias) / 100
  const historiaTotal = (historia * ponderaciones.historia) / 100

  const resultado =
    rankingTotal +
    nemTotal +
    lectoraTotal +
    mate1 +
    mate2 +
    cienciasTotal +
    historiaTotal

  return resultado.toFixed(2)
}
const tabs = ref<'datos' | 'carreras'>('datos')
</script>

<template>
  <div class="w-full ml-auto mr-auto space-y-4">
    <div>
      <h1 class="text-2xl font-bold">Calculadora Carreras</h1>
      <p class="text-muted-foreground">Calculadora Ponderaciones</p>
    </div>

    <Tabs default-value="resumen" v-model="tabs">
      <TabsList>
        <TabsTrigger value="datos"> Mis Datos </TabsTrigger>
        <TabsTrigger value="carreras"> Carreras </TabsTrigger>
      </TabsList>
      <!--   <TabsContent value="resumen"> </TabsContent>
        <TabsContent value="chat"> </TabsContent> -->
    </Tabs>

    <Card v-if="tabs === 'carreras'">
      <h1 class="text-2xl font-bold">Carreras</h1>
      <Table>
        <TableHeader>
          <TableRow>
            <TableHead>Carrera</TableHead>
            <TableHead>Resultado</TableHead>
            <TableHead>Último Matriculado</TableHead>
            <TableHead>NEM</TableHead>
            <TableHead>RANKING</TableHead>
            <TableHead>LECTORA</TableHead>
            <TableHead>MATE 1</TableHead>
            <TableHead>MATE 2</TableHead>
            <TableHead>CIENCIAS</TableHead>
            <TableHead>HISTORIA</TableHead>
            <!-- <TableHead>Último Seleccionado</TableHead> -->
          </TableRow>
        </TableHeader>

        <TableBody>
          <TableRow v-for="(c, k) in universidades" :key="k">
            <TableCell>{{ c.carrera }} - {{ c.universidad }}</TableCell>
            <TableCell class="font-bold">{{
              calcularResultado(c.ponderaciones)
            }}</TableCell>
            <TableCell class="font-bold">{{ c.ultimo_matriculado }}</TableCell>
            <TableCell>{{ c.ponderaciones.nem }}%</TableCell>
            <TableCell>{{ c.ponderaciones.ranking }}%</TableCell>
            <TableCell>{{ c.ponderaciones.lectora }}%</TableCell>
            <TableCell>{{ c.ponderaciones.mate_1 }}%</TableCell>
            <TableCell>{{ c.ponderaciones.mate_2 }}%</TableCell>
            <TableCell>{{ c.ponderaciones.ciencias }}%</TableCell>
            <TableCell>{{ c.ponderaciones.historia }}%</TableCell>
            <!--  <TableCell>{{ c.ultimo_seleccionado }}</TableCell> -->
          </TableRow>
        </TableBody>
      </Table>
      <!-- <div>
        <Label>Carrera</Label>
        <Select v-model="datos.universidad">
          <SelectTrigger class="rounded-full">
            <SelectValue placeholder="Selecciona una Universidad" />
          </SelectTrigger>
          <SelectContent class="rounded-2xl">
            <SelectGroup>
              <SelectLabel>Universidades</SelectLabel>
              <SelectItem
                v-for="(c, k) in universidades"
                :key="k"
                :value="`${k}`"
              >
                {{ c.carrera }}
              </SelectItem>
            </SelectGroup>
          </SelectContent>
        </Select>

        <div v-if="selectedUniversidad">
          {{ selectedUniversidad.carrera }}
        </div>
      </div> -->
    </Card>
    <Card v-if="tabs === 'datos'">
      <h1 class="text-2xl font-bold">Mis Datos</h1>
      <div>
        <Label>NEM</Label>
        <Input placeholder="NEM" v-model.number="datos.nem" />
      </div>
      <div>
        <Label>RANKING</Label>
        <Input placeholder="RANKING" v-model.number="datos.ranking" />
      </div>

      <div>
        <Label>LECTORA</Label>
        <Input placeholder="LECTORA" v-model.number="datos.lectora" />
      </div>

      <div>
        <Label>MATEMÁTICAS 1</Label>
        <Input placeholder="MATEMÁTICAS 1" v-model.number="datos.mate_1" />
      </div>

      <div>
        <Label>MATEMÁTICAS 2</Label>
        <Input placeholder="MATEMÁTICAS 2" v-model.number="datos.mate_2" />
      </div>

      <div>
        <Label>CIENCIAS</Label>
        <Input placeholder="CIENCIAS" v-model.number="datos.ciencias" />
      </div>
    </Card>
  </div>
</template>

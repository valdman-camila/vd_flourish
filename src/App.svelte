<!-- Script JS -->
<script>
  /* Importamos d3 para tenerlo disponible */
  import * as d3 from "d3"

  // Diversity of Lepidoptera in each faunal region vs Estimated number of species (Wikipedia)
  let dataLepidopteraRegion = [
    {region: "Paleártico", cantidad: 22465},
    {region: "Neártico", cantidad: 11532},
    {region: "Neotrópico", cantidad: 44791},
    {region: "Etiópica", cantidad: 20491},
    {region: "Indo-australiana", cantidad: 47286},
  ]

  // encontrar el minimo y usarlo para construir el grafico
  let cantidadMinima = dataLepidopteraRegion[0].cantidad * 0.22;

  for (let i = 1; i < dataLepidopteraRegion.length; i++) {
    if (dataLepidopteraRegion[i].cantidad < cantidadMinima) {
      cantidadMinima = dataLepidopteraRegion[i].cantidad * 0.22;
    }
  }

  function calculateMedian(data) {
    // armar una lista ordenada con todos los numeros
    let amounts = data.map(item => item.cantidad).sort((a, b) => a - b);

    // obtener el indice medio 
    let middleIndex = Math.floor(amounts.length / 2);

    // si la cantidad de numeros es par devuelve el promedio de dos numeros del medio. si es impar, devuelve el numero medio
    if (amounts.length % 2 === 0) {
        return (amounts[middleIndex - 1] + amounts[middleIndex]) / 2;
    } else {
        return amounts[middleIndex];
    }
  }

  let medianCantidad = calculateMedian(dataLepidopteraRegion);

  let altura = d3
    .scaleLinear()
    .domain([0, d3.max(dataLepidopteraRegion, d => d.cantidad)])
    .range([0, 500])

</script>

<main>
  <!-- Mi Proyecto -->
  <div class="header">
    <img
      src="./images/moth-clipart-original.png"
      width="200"
      alt="Polilla"
    />
    <h3 class="headline">
      <b>Lepidoptera</b>
      Como la diversidad de esta orden esta siendo afectada
    </h3>
    <p>
      De las aproximadamente 174,250 especies de lepidópteros descritas hasta 2007, se estima que las mariposas y los hespéridos comprenden alrededor de 17,950, mientras que el resto son polillas. Los lepidópteros están distribuidos en todas las regiones faunísticas excepto la Antártida, pero en áreas como el Neotrópico y la región Indo-australiana, la riqueza de especies es particularmente alta, con mariposas y polillas adaptadas a ecosistemas muy específicos. Sin embargo, esta diversidad se ha visto cada vez más amenazada por la destrucción de los hábitats a causa de la actividad humana. Esto se puede corroborar, por un lado, gracias al índice de Simpson, que se interpreta como la probabilidad de un encuentro intraespecífico (que dos individuos tomados al azar de una comunidad sean de la misma especie), lo que significa que mientras más alta es esta probabilidad, menos diversa es la comunidad. A su vez, el índice de Shannon-Wiener (H') mide la probabilidad de seleccionar todas las especies en la proporción con que existen en la población, e incrementa a medida que aumenta el número de especies y los individuos se distribuyen homogéneamente. El índice de Shannon prioriza la riqueza mientras que el índice de Simpson prioriza la uniformidad. En este sentido, podemos ver cómo, a través del tiempo, la diversidad de los lepidópteros ha disminuido a la vez que la actividad humana ha afectado en el medioambiente.
    </p>
  </div>   

  <div class="graph_container">
    <!-- Grafico con polillas -->
    <div class="container">
      {#each dataLepidopteraRegion as item}
        <div class="item_wrapper">
          {#if item.cantidad > medianCantidad}
            <img class="bigMoth" src="./images/moth-illustration-by-Vexels.svg" alt="silkmoth" style="height: {(altura(item.cantidad - cantidadMinima) * 2)}px"/>
            <img class="Leaf" src="./images/double_leaf.svg" alt="big leaf" style="height:{altura(item.cantidad - cantidadMinima) * 2}px" />
          {:else}
            <img class="smallMoth" src="./images/grey-moth-illustration-by-Vexels.svg" alt="common moth" style="height:{altura(item.cantidad - cantidadMinima) * 2}px"/>
            <img class="Leaf" src="./images/double_leaf.svg" alt="small leaf" style="height:{altura(item.cantidad - cantidadMinima) * 2}px"/>
          {/if}
          <p class="numMoth">{item.cantidad}</p>
          <div class="column2" style="height: {altura(item.cantidad)}px"></div>
        </div>
      {/each}
    </div>
  
    <div class="region-labels">
      {#each dataLepidopteraRegion as item}
        <div class="region-label" style="width: calc(100% / {dataLepidopteraRegion.length})">
          <p class="counMoth">{item.region}</p>
        </div> 
      {/each}
    </div>
  </div>
  <br />

  <div class="chart-wrapper">
    <div 
      class="flourish-embed flourish-chart" 
      data-src="visualisation/22635054">
      <script src="https://public.flourish.studio/resources/embed.js"></script>
      <noscript>
      <img src="https://public.flourish.studio/visualisation/22635054/thumbnail" 
       width="100%" 
       alt="chart visualization" />
      </noscript>
    </div>
  </div>

  <br />

  <div class="flourish-embed flourish-chart" data-src="visualisation/22635962"><script src="https://public.flourish.studio/resources/embed.js"></script><noscript><img src="https://public.flourish.studio/visualisation/22635962/thumbnail" width="100%" alt="chart visualization" /></noscript></div>
 
  <br />
  <hr>
  <br />

  <div class="footer">
    <p>Creado por <a href="https://www.linkedin.com/in/camila-valdman-3241542a2/">Camila Nicole Valdman</a> para la materia de Visualización de Datos.</p>
  </div>

</main>

<style>
  @import url("https://fonts.googleapis.com/css2?family=DM+Sans:ital,opsz,wght@0,9..40,100..1000;1,9..40,100..1000&display=swap");

  * {
    font-family: "DM Sans", sans-serif;
  }

  .footer {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    margin-bottom: 50px;
    margin-top: 60px;
  }

  .header {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    margin-top: 50px;
    margin-bottom: 60px;
  }

  .headline {
    font-size: 30px;
    line-height: 1.2;
    font-weight: normal;
    text-align: center;
    margin: 20px;
  }

  .headline b {
    display: block;
    font-size: 35px;
    margin-bottom: 5px;
  }

  .chart-wrapper {
    max-width: 1100px;
    margin: auto;
    margin-bottom: 130px;
    margin-top: 130px;
  }

  .graph_container {
    position: relative;
    margin-bottom: 60px; 
  }

  .container {
    display: flex;
    justify-content: space-between;
    gap: 10px;
    height: 550px;
    position: relative;
  }

  .region-labels {
    display: flex;
    justify-content: space-between;
    position: absolute;
    bottom: -50px;
    width: 100%;
    left: 0;
  }

  .region-label {
    text-align: center;
    padding: 0 5px;
  }

  .item_wrapper {
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    align-items: center;
    gap: 10px;
    flex: 1;
    position: relative;
    height: 100%;
  }

  .column2 {
    width: 20px;
    height: 100%;
    border-radius: 30px;
    background-color: rgb(70, 126, 2);
  }

  .numMoth {
    font-size: 24px;
    font-weight: bold;
    color: #906900;
  }

  .counMoth {
    font-size: 24px;
    font-weight: bold;
    color: #906900;
    text-align: center;
    margin: 0;
  }

  .bigMoth{
    position: absolute;
    z-index: 1;
    width: 200px;
    height: 100%;
    transform: scale(0.9);
  }

  .smallMoth{
    position: absolute;
    z-index: 1;
    width: 150px;
    height: 100%;
    transform: scale(0.9);
  }

  .Leaf{
    position: absolute;
    z-index: 0;
    width: 200px;
    height: 100%;
    transform: scale(0.8);
  }

  p {
    font-size: 18px;
  }
</style>

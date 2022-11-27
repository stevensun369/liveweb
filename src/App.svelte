<script>
  import * as L from 'leaflet';
  import 'leaflet/dist/leaflet.css';
	import axios from 'axios' 
  let map;
	let marker

  function createMap(container) {
    let m = L.map(container).setView([lat, long], 13);

    L.tileLayer(
      'https://{s}.basemaps.cartocdn.com/rastertiles/voyager/{z}/{x}/{y}{r}.png',
      {
        attribution: `&copy;<a href="https://www.openstreetmap.org/copyright" target="_blank">OpenStreetMap</a>,
          &copy;<a href="https://carto.com/attributions" target="_blank">CARTO</a>`,
        subdomains: 'abcd',
        maxZoom: 18,
      }
    ).addTo(m);

		marker = L.marker([lat, long]).addTo(m);

    return m;
  }

  function mapAction(container) {
    map = createMap(container);
    return {
      destroy: () => {
        map.remove();
      },
    };
  }

	let lat = 45.7574972
	let long = 21.2263437

	async function getLocation() {
		const {data} = await axios.get(
			'https://api.elmtree.ro/'
		)

		console.log(data.long)

		marker.setLatLng([data.lat, data.long])
		console.log(marker)
	}

	setInterval(async () => {
		await getLocation()
	}, 1000)
</script>

<svelte:head>
   <link rel="stylesheet" href="https://unpkg.com/leaflet@1.6.0/dist/leaflet.css"
   integrity="sha512-xwE/Az9zrjBIphAcBb3F6JVqxf46+CDLwfLMHloNu6KEQCAWi6HcDUbeOfBIptF7tcCzusKFjFw2yuvEpDL9wQ=="
   crossorigin=""/>
</svelte:head>

<div style="height:100vh;width:100%" use:mapAction />

<!-- <script>
	import {LeafletMap, TileLayer} from 'svelte-leafletjs';

	const mapOptions = {
			center: [1.364917, 103.822872],
			zoom: 11,
	};
	const tileUrl = "https://{s}.basemaps.cartocdn.com/rastertiles/voyager/{z}/{x}/{y}{r}.png";
	const tileLayerOptions = {
			minZoom: 0,
			maxZoom: 20,
			maxNativeZoom: 19,
			attribution: "© OpenStreetMap contributors",
	};

	let leafletMap;
</script>

<div class="example">
	<LeafletMap bind:this={leafletMap} options={mapOptions}>
			<TileLayer url={tileUrl} options={tileLayerOptions}/>
	</LeafletMap>
</div> -->
<script>
	import TwentyEight from './TwentyEight.svelte';
	import ShowByTag from './ShowByTag.svelte';
	import ShowSelected from './ShowSelected.svelte';
  import { DateTime } from 'luxon';
  // import '@github/details-dialog-element';
  // import '../../../node_modules/@material/mwc-button/mwc-button.js';

  // import { WiredButton, WiredInput } from "wired-elements"

	// Firebase App (the core Firebase SDK) is always required and
	// must be listed before other Firebase SDKs
	import firebase from 'firebase/app/dist/index.cjs.js';

	// Add the Firebase services that you want to use
	// import "firebase/auth";
	import 'firebase/firestore/dist/index.node.cjs.js';

	const config = {
		apiKey: 'AIzaSyCo1_tSUtuq8bmuuGJ01AdvpkdUB4twU-Y',
		authDomain: 'dev-2d025.firebaseapp.com',
		projectId: 'dev-2d025',
		storageBucket: 'dev-2d025.appspot.com',
		messagingSenderId: '155757852699',
		appId: '1:155757852699:web:889c9bdf1423f1c082e0be',
		measurementId: 'G-209RZ60MX4'
	};

	// Initialize Firebase
	!firebase.apps.length ? firebase.initializeApp(config) : firebase.app();

	var db = firebase.firestore();

function openDialog() {
  document.querySelector('wired-dialog').setAttribute('open', '');
}
function closeDialog() {
  document.querySelector('wired-dialog').removeAttribute('open');
}

	function onClick_addHabit() {
    // alert('index onClick_addHabit()')
    
    const habitName = document.querySelector('[name=habitName]').value;
    const tagsString = document.querySelector('[name=tags]').value;
    const tagsArray = tagsString.split(',');

    if (!habitName) {
      alert("Please enter a name for the TwentyEight");
      return null;
    }

    let obj = {};
    
		// DateTime.now().toISODate()
		const dt = DateTime.now();

		for (let i = 0; i < 28; i++) {
			obj[dt.plus({ day: i }).toISODate()] = {
        isCompleted: false,
        dateTimeCompleted: null,
			};
		}

		db.collection('twentyeights')
			.add({
				startDate: DateTime.now().toISODate(), // todo might change
        days: obj,
        name: document.querySelector('[name=habitName]').value,
        tags: tagsArray
				// last: 'Lovelace',
				// born: 1815
			})
			.then((docRef) => {
        // console.log('Document written with ID: ', docRef.id);
        

        db.collection("twentyeights").doc( docRef.id ).update({
            id: docRef.id
        })
			})
			.catch((error) => {
				console.error('Error adding document: ', error);
      });
      document.querySelector('[name=habitName]').value = null;
      document.querySelector('[name=tags]').value = null;

	}


	// var docRef = db.collection('twentyeights').doc(id);
	var collection28 = db.collection('twentyeights').get();

	let data = {};

	collection28
  .then((querySnapshot) => {


    querySnapshot.forEach((twentyEight) => {
        // doc.data() is never undefined for query doc snapshots
        // console.log(twentyEight.id, " => ", twentyEight.data());
        data[twentyEight.id] = twentyEight.data()
    });

    // console.log("data", data);
    // console.log(typeof data)
});

function filterDataByTag(data, tag) {
  // return data.filter(twentyEight => twentyEight.tags.includes(tag));

  // Object.entries(data).filter(([key, arr]) => console.log('blah', arr) );
  // console.log("woof" );

  // debugger;

  let newObject = Object.assign({}, data);

  Object.entries(data).filter( ([key, arr]) => { 
    // console.log("arr.tags", arr.tags);

    if (!arr.tags.includes(tag)) {
      delete newObject[key];
    }
  });

  return newObject;
}
</script>

<section>
  <!-- <mwc-button on:click={onClick_addHabit} id="myButton" label="+ TWENTYEIGHT" raised></mwc-button> -->

  <!-- <mwcButton on:click={onClick_addHabit} id="myButton" label="+ TWENTYEIGHT" raised></mwcButton> -->

  <wired-button>Click Me</wired-button>
  <wired-button on:click={openDialog}>open dialog</wired-button>

  <wired-dialog>
    <p>
      Dialog content here
    </p>
    <div style="text-align: right; padding: 30px 16px 16px;">
      <wired-button id="closeDialog" on:click={closeDialog}>Close dialog</wired-button>
    </div>
  </wired-dialog>

  <button on:click={onClick_addHabit}>+ TWENTYEIGHT</button> 
  <input type="text" name="habitName" placeholder="habit name" />
  <input type="text" name="tags" placeholder="tags (separated by commas, no spaces after commas)" />

  <!-- <details>
    <summary>Open dialog</summary>
    <details-dialog>
      Modal content
      <button type="button" data-close-dialog>Close</button>
    </details-dialog>
  </details> -->


  <!-- <details class="details-reset details-with-dialog mt-4">
    <summary class="btn" role="button">Dialog</summary>
    <details-dialog class="details-dialog anim-fade-in fast wide" aria-label="Dialog" role="dialog" aria-modal="true" tabindex="-1">
      <div class="Box d-flex flex-column">
        <div class="Box-header">
          Title
        </div>
        <div class="Box-body overflow-auto">
          Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
        </div>
        <div class="Box-footer">
          <button type="button" class="btn btn-block" data-close-dialog="">Close</button>
        </div>
      </div>
    </details-dialog>
  </details> -->
  
	<!-- {DateTime.now().toISODate()} -->
	<br />
  <br />
  <br />
  <br />

  {#if Object.keys(data).length > 0}

  

  <!-- <ShowByTag tag="Alaska Airlines" db={db} data={data}>
  </ShowByTag>
  <ShowByTag tag="stretches" db={db} data={data} >
  </ShowByTag>

  <ShowByTag tag="Fabricator Studio" db={db} data={data} >
  </ShowByTag>

  <ShowByTag tag="chores" db={db} data={data} >
  </ShowByTag>
  <ShowByTag tag="daily reading" db={db} data={data} >
  </ShowByTag> -->




  <ShowSelected tag="morning routine" db={db} data={filterDataByTag(data, 'morning routine')}>
  </ShowSelected>

  <ShowSelected tag="Alaska Airlines" db={db} data={filterDataByTag(data, 'Alaska Airlines')}>
  </ShowSelected>

  <ShowSelected tag="stretches" db={db} data={filterDataByTag(data, 'stretches')}>
  </ShowSelected>

  <ShowSelected tag="Fabricator Studio" db={db} data={filterDataByTag(data, 'Fabricator Studio')}>
  </ShowSelected>

  <ShowSelected tag="daily reading" db={db} data={filterDataByTag(data, 'daily reading')}>
  </ShowSelected>

  <ShowSelected tag="relationships" db={db} data={filterDataByTag(data, 'relationships')}>
  </ShowSelected>

  <ShowSelected tag="test" db={db} data={filterDataByTag(data, 'test')}>
  </ShowSelected>





  <!-- <ShowSelected tag="" db={db} data={filterDataByTag(data, '')}>
  <ShowSelected tag="" db={db} data={filterDataByTag(data, '')}>
  <ShowSelected tag="" db={db} data={filterDataByTag(data, '')}> -->



  {/if}


  
</section>

<style>
  [name="tags"] {
    width: 400px;
  }
</style>
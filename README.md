
# Orbit Reel

### Installation

Reference or download the library on the CDN: https://orbitreelcdn.s3.amazonaws.com/stable/orbit-reel.js

Alternatively, you may have been given a customized turnkey script by your sales contact. Use that instead.

HTML:
```
<head>
    <script src="https://orbitreelcdn.s3.amazonaws.com/stable/orbit-reel.js" />
</head>
```

### Basic Usage

A quick snippet to get you started:
```
OrbitReel.generate(document.getElementById('YOUR_CONTAINER_ROOT'), {
  token: 'YOUR_TOKEN_HERE',
  contentLoader: 'ursula',
  includeStatusFilter: true,
  includeMoveInFilter: true,
});
```
The library requires that both an container reference and an appropriate orbit reel token be passed in. The rest of the API surface layer is just optional config details.

### Generation Type Signature
```
OrbitReel.generate(element: HTMLElement, config: IConfig, callback?: (reel: Reel, destructor: () => void))
```

### Manual Memory Cleanup and Direct Reel Control

These are optional services that can be provided to your application. They can be obtained via callback through an optional third parameter passed to the OrbitReel.generate function. 

For versions 0.56+, manual memory cleanup is only necessary if you decide to re-use the same DOM element. In any other case, simply `remove()` the DOM element you initially passed to the generate function, and the application will automatically dismount.

An example showcasing:
	-obtaining access to these optional services
	-using reel direct access to add an event listener for clicked units
	-manually disposing the app

```
const {dispose, reel} = await (function() {
	return new Promise((resolve) => {
		OrbitReel.generate(element, config, (reel,dispose) => {
			resolve({dispose, reel});
		});
	});
})();
//...
reel.addEventListener('unit-clicked', (unit) => console.log(`${unit.name} was clicked!`));
//...
dispose();
```

Full type signature for the Reel object can be found [here](https://github.com/evolution-virtual/orbit-reel-public/blob/master/README.md#reel)


### Config

A fully typed interface containing all current config variables:

```
{
  includeActionBar: boolean;
  actionBarType: 'default';
  includeBedroomFilter: boolean;
  includeBathroomFilter: boolean;
  includeStatusFilter: boolean;
  includePriceFilter: boolean;
  includeSearchFilter: boolean;
  includeMoveInFilter: boolean;
  token: string;
  assetStore: string;
  isPortraitCompatible: boolean;
  debugMode: boolean;
  labelColor: string;
  labelColor1: string;
  labelColor2: string;
  labelColor3: string;
  labelColor4: string;
  hideAvailableUnits: boolean;
  hideUnavailableUnits: boolean;
  includeUnitPopup: boolean;
  unitPopupType: 'default';
  initialFrameNumber: number;
  contentLoader: 'ursula' | 'globalHawk' | 'custom';
  ursulaToken: string;
  globalHawkToken: string;
  ursulaIsMultipart: boolean;
  sitemapSrc: string;
  includeIntroduction: boolean;
  watermarkType: 'default';
  includeWatermark: boolean;
  introductionType: 'default';
  includeLandingPage: boolean;
  landingType: 'default';
  allowFavoriting: boolean;
  ghEndpoint: string;
  units: CORE.Type.IUnit[];
  mode: CORE.Type.IApplicationMode;
  priceFormatter: (price: number) => string;
   disableTransitions: boolean;
  hideUnavailableUnitDetails: boolean;
  translations: {
    unitOverlayStatus: string;
    unitOverlayPrice: string;
    unitOverlayExteriorSize: string;
    threeDUnitSearch: string;
    buildingAmenities: string;
    twoDUnitSearch: string;
    filterUnitSearch: string;
  };
  unitOverlayFields: {
    disableStatus: boolean;
    disableBedrooms: boolean;
    disableBathrooms: boolean;
    disableAvailability: boolean;
    disableExteriorSize: boolean;
    disableInteriorSize: boolean;
    disableTotalSquareFeet: boolean;
    disablePricing: boolean;
    disableView: boolean;
  };
  logoSrc: string;
  watermarkSrc: string;
}
```

Breakdown of Configuration Variables

```
export type IApplicationMode =
  | 'units'
  | 'amenities'
  | 'gallery'
  | 'siteMap'
  | 'floors';
```

If you find that any of these config variables don't work as you expect, or there's behavior in the app that you'd like to configure more precisely, send us an email!

### Events
The library provides the following events:
    'unit-clicked' -> when a unit label is clicked
    'unit-enter' -> when the user moves their cursor over a unit label
    'unit-leave' -> when the user moves their cursor outside of a unit label that's already fired a 'unit-enter' event
    'floor-clicked' -> when the user clicks a floor in the 3D floor view 
    'floor-enter' -> when the user moves their cursor over a floor in the 3D floor view
    'floor-leave' -> when the user moves their cursor outside of a floor in the 3D floor view
    'amenity-clicked' -> when the user clicks an amenity in the 3D amenity view
    
To subscribe to any of these events, you can call 'addEventListener' on the reel object ('removeEventListener' is the inverse).

A reference to the reel object can be obtained through the constructor. There's an optional third parameter on the constructor that takes in a callback for receiving the reel object. Example:
    
    OrbitReel.generate(container, 
        {token: 'YOUR_TOKEN', ...}
        function(reel, destructor) {
            //Reel is received here
            reel.addEventListener('unit-clicked', function(unit) {
                alert(unit.name + 'was clicked!');
            }
        }
    )

### Reel
```
class Reel {
	addEventListener:  (type: ListenerTypes, listener:  (data: any)  => void)  => void;
	removeEventListener:  (type: ListenerTypes, listener:  (data: any)  => void)  => void;
	set mode(mode: IApplicationMode);
	moveToReel:  (token: string, force?: boolean, viewAngle?: number)  => void;
	handleSitemapTransition:  (viewAngle: number, communityToken: string)  => void;
	playTransition:  (name: string, snap?: boolean)  => Promise<void>;
	hoverUnit:  (unitName: string)  => void;
	unhoverUnit:  (unitName: string)  => void;
	resetReel:  ()  => void;
	exitTransition:  ()  => void;
	set  unitFilter(filter:  (unit: IUnit)  => boolean);
	handleLabelClicked:  (unit: IUnit)  => void;
	handleLabelEnter:  (unit: IUnit)  => void;
	handleSetMousePosition:  (x: number, y: number)  => void;
	handleLabelLeave:  (unit: IUnit)  => void;
	handleFloorClicked:  (floor: number)  => void;
	handleFloorEnter:  (floor: number)  => void;
	handleFloorLeave:  (floor: number)  => void;
	handleAmenityClicked:  (amenity: IClickableRegion)  => void;
	get  zoom(): number;
	set  zoom(zoom: number);
	get  transitions(): ITransition[];
	set  renderUnits(shouldRenderUnits: boolean);
	set  renderAmenities(shouldRenderAmenities: boolean);
	set  renderSitemap(shouldRenderSitemap: boolean);
	set  renderFloors(shouldRenderFloors: boolean);
	get  renderUnits(): boolean;
	get  renderAmenities(): boolean;
	get  renderSitemap(): boolean;
	get  renderFloors(): boolean;
}
```

    
## Advanced Recipes

### Loading custom property data
#### Use case: 
You have property data that needs to be kept in sync with an external API, and isn't covered under our own Ursula or GlobalHawk platforms.
#### Recipe:
  
    const externalUnits = await getUnitsFromExternalApi();
    
    OrbitReel.generate(container, 
        {
            token: 'YOUR TOKEN', 
            units: externalUnits
        }
    );
    
### Custom price formatting
#### Use case: 
You find the standard way we format price to be too limiting, and want an alternative method for expressing price info.

#### Recipe:
    function formatPrice(price) {
        if (price > 10000000) {
            return 'Call for assistance';
        }
        
        return `$${Math.round(price)}`
    }
    
    OrbitReel.generate(container, {
        token: 'YOUR_TOKEN',
        priceFormatter: formatPrice
    });
    


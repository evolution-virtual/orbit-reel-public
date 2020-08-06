# Orbit Reel

### Installation

Reference or download the library on the CDN: https://orbitreelcdn.s3.amazonaws.com/stable/orbit-reel.js

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
        function(reel) {
            //Reel is received here
            reel.addEventListener('unit-clicked', function(unit) {
                alert(unit.name + 'was clicked!');
            }
        }
    )
    
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
    


import React, { useState, useMemo } from 'react';
import { 
  Compass, MapPin, Calendar, Heart, Search, DollarSign, Utensils, 
  Info, ShieldAlert, Sparkles, Navigation, Sun, Waves, Hotel, 
  CheckCircle, ArrowRight, ChevronRight, X, ExternalLink, Bookmark, 
  Layers, Users, Anchor, BookOpen, Camera, Filter, Award, Send
} from 'lucide-react';

// --- DATA STRUCTURES ---

const ISLANDS_DATA = [
  {
    id: 'bongao',
    name: 'Bongao',
    tagline: 'The vibrant gateway & commercial heartbeat of Tawi-Tawi',
    municipality: 'Bongao',
    type: 'Main Gateway',
    image: 'https://images.unsplash.com/photo-1507525428034-b723cf961d3e?auto=format&fit=crop&w=800&q=80',
    description: 'Bongao is the administrative and economic center of Tawi-Tawi. Dominated by the majestic sacred peak of Bud Bongao, this island features bustling seafood markets, historical piers, and stunning coastal views.',
    highlights: ['Bud Bongao Peak', 'Bihing Tahik', 'Boloboc Cave', 'Chinese Pier', 'Bongao Public Market'],
    attractions: [
      {
        id: 'bud-bongao',
        name: 'Bud Bongao',
        type: 'Sacred Mountain & Nature Park',
        duration: '3 - 4 hours',
        fee: '₱20 - ₱50 (Conservation Fee)',
        image: 'https://images.unsplash.com/photo-1464822759023-fed622ff2c3b?auto=format&fit=crop&w=800&q=80',
        desc: 'A sacred mountain rising over 300 meters above sea level. Revered by local communities, it is home to friendly long-tailed macaques and offers sweeping 360-degree views of the archipelago.',
        reminders: 'Bring bananas for the monkeys, dress respectfully, avoid littering, and follow marked stone steps.'
      },
      {
        id: 'bihing-tahik',
        name: 'Bihing Tahik',
        type: 'Coastal Resort & Beach Front',
        duration: '1 - 2 hours',
        fee: 'Free entry (Minimal cottage rentals)',
        image: 'https://images.unsplash.com/photo-1519046904884-53103b34b206?auto=format&fit=crop&w=800&q=80',
        desc: 'Meaning "By the Sea" in local Sama, Bihing Tahik offers tranquil beachside relaxation, picturesque sunset views, and fresh sea breezes close to the town center.',
        reminders: 'Great spot for evening relaxed dining and photography.'
      },
      {
        id: 'boloboc-cave',
        name: 'Boloboc Cave',
        type: 'Archaeological Heritage Site',
        duration: '1 hour',
        fee: 'Free / Local Guide Suggested',
        image: 'https://images.unsplash.com/photo-1509316975850-ff9c5deb0cd9?auto=format&fit=crop&w=800&q=80',
        desc: 'An important archaeological rock shelter where prehistoric tools, shell artifacts, and ancient human remains dating back over 8,000 years were discovered.',
        reminders: 'Wear comfortable walking shoes and exercise caution on slippery rocks inside.'
      },
      {
        id: 'chinese-pier',
        name: 'Chinese Pier (Stilt Market)',
        type: 'Cultural & Historic Waterfront',
        duration: '1 hour',
        fee: 'Free',
        image: 'https://images.unsplash.com/photo-1544551763-46a013bb70d5?auto=format&fit=crop&w=800&q=80',
        desc: 'A historic trading hub featuring colorful stilt houses over the water where local fishermen, traders, and boat captains gather daily.',
        reminders: 'Always ask permission before taking close-up portraits of residents.'
      }
    ],
    transport: 'Tricycles are available throughout town (₱15-₱30 per route). Speedboats and passenger ferries depart daily for surrounding islands.',
    stayInfo: 'Widely available (Hotels, Inns, Guesthouses).'
  },
  {
    id: 'sanga-sanga',
    name: 'Sanga-Sanga',
    tagline: 'Transportation hub & tranquil coastal countryside',
    municipality: 'Bongao',
    type: 'Transit & Coastal Hub',
    image: 'https://images.unsplash.com/photo-1510414842594-a61c69b5ae57?auto=format&fit=crop&w=800&q=80',
    description: 'Home to the primary Sanga-Sanga Airport (TWT), this island connects visitors to mainland Mindanao and the rest of the country while offering serene beaches and mangrove rivers.',
    highlights: ['Sanga-Sanga Airport', 'White Sand Beaches', 'Mindanao State University Campus Grounds'],
    attractions: [
      {
        id: 'sanga-beaches',
        name: 'Sanga-Sanga Coastal Stretch',
        type: 'Beach & Coastal Line',
        duration: '2 hours',
        fee: 'Free',
        image: 'https://images.unsplash.com/photo-1506929562872-bb421503ef21?auto=format&fit=crop&w=800&q=80',
        desc: 'Clear shallow waters lining the perimeter of Sanga-Sanga Island, perfect for quick stopovers right after landing at the airport.',
        reminders: 'Respect private properties along the shore.'
      }
    ],
    transport: 'Connected to Bongao Island via a short bridge. Tricycles and private vehicles cross continuously.',
    stayInfo: 'Limited local inns; most travelers stay in nearby Bongao town proper.'
  },
  {
    id: 'simunul',
    name: 'Simunul',
    tagline: 'Cradle of Islamic Faith in the Philippines',
    municipality: 'Simunul',
    type: 'Historical & Religious Landmark',
    image: 'https://images.unsplash.com/photo-1564507592333-c60657eea523?auto=format&fit=crop&w=800&q=80',
    description: 'Simunul holds immense historical significance as the site of the first mosque built in the Philippines by Arab trader Sheik Karimul Makhdum in 1380 AD.',
    highlights: ['Sheik Karimul Makhdum Mosque', 'Tampakan Beach', 'Papag Beach'],
    attractions: [
      {
        id: 'makhdum-mosque',
        name: 'Sheik Karimul Makhdum Mosque',
        type: 'National Cultural Treasure / Heritage Site',
        duration: '1 - 2 hours',
        fee: 'Donations Appreciated',
        image: 'https://images.unsplash.com/photo-1584551246679-0daf3d275d0f?auto=format&fit=crop&w=800&q=80',
        desc: 'The oldest surviving Islamic place of worship in the Philippines. Features original wooden pillars preserved inside the modern structure.',
        reminders: 'Modest attire required (heads covered for females, long pants/skirts, shoes removed inside).'
      }
    ],
    transport: '1-hour passenger boat ride from Bongao Chinese Pier.',
    stayInfo: 'Homestays and municipal guesthouses.'
  },
  {
    id: 'sibutu',
    name: 'Sibutu',
    tagline: 'Land of traditional boat builders and rich maritime craftsmanship',
    municipality: 'Sibutu',
    type: 'Craftsmanship & Cultural Hub',
    image: 'https://images.unsplash.com/photo-1540555700478-4be289fbecef?auto=format&fit=crop&w=800&q=80',
    description: 'Famed for master boat-builders who craft traditional wood kumpit and lepa vessels by hand without formal blueprints. Features wild beaches and deep ocean channels.',
    highlights: ['Traditional Boatyards', 'Saluag Island Light House', 'Sibutu Natural Springs'],
    attractions: [
      {
        id: 'sibutu-boat-building',
        name: 'Sibutu Master Boatyards',
        type: 'Living Cultural Heritage',
        duration: '2 hours',
        fee: 'Free (Ask permission from craftsmen)',
        image: 'https://images.unsplash.com/photo-1520250497591-112f2f40a3f4?auto=format&fit=crop&w=800&q=80',
        desc: 'Witness generations of artisans handcrafting large wooden boats with immense precision.',
        reminders: 'Respect the artisans while they work.'
      }
    ],
    transport: '2 to 3-hour boat journey south from Bongao.',
    stayInfo: 'Basic community homestays.'
  },
  {
    id: 'sitangkai',
    name: 'Sitangkai',
    tagline: 'The "Venice of the South" stilt-house aquatic town',
    municipality: 'Sitangkai',
    type: 'Maritime Stilt Community',
    image: 'https://images.unsplash.com/photo-1518509562904-e7ef99cdcc86?auto=format&fit=crop&w=800&q=80',
    description: 'The southernmost municipality of the Philippines. A picturesque town built entirely over water, where wide sea canals replace streets and boats replace motor vehicles.',
    highlights: ['Canal Waterways', 'Seaweed Farming Farms', 'Badjao & Sama Settlements'],
    attractions: [
      {
        id: 'sitangkai-canals',
        name: 'Sitangkai Grand Canal',
        type: 'Aquatic Town Streetscape',
        duration: 'Half Day',
        fee: 'Boat Rental Varies',
        image: 'https://images.unsplash.com/photo-1559827291-72ee739d0d9a?auto=format&fit=crop&w=800&q=80',
        desc: 'Ride traditional wooden canoes through active waterways lined with colorful houses and floating markets.',
        reminders: 'Coordinate in advance with local municipal tourism officers.'
      }
    ],
    transport: '3 to 4-hour speedboat ride from Bongao.',
    stayInfo: 'Limited homestays available upon municipal clearance.'
  },
  {
    id: 'turtle-islands',
    name: 'Turtle Islands',
    tagline: 'Protected sanctuary for Green Sea Turtles & rich biodiversity',
    municipality: 'Turtle Islands',
    type: 'Marine Sanctuary & Protected Area',
    image: 'https://images.unsplash.com/photo-1544551763-46a013bb70d5?auto=format&fit=crop&w=800&q=80',
    description: 'A major nesting ground for Green and Hawksbill turtles in Southeast Asia. Strict conservation measures protect these remote ecosystems.',
    highlights: ['Turtle Nesting Beaches', 'Taganak Island', 'Baguan Island Wildlife Sanctuary'],
    attractions: [
      {
        id: 'baguan-sanctuary',
        name: 'Baguan Turtle Nesting Site',
        type: 'Protected Marine Reserve',
        duration: 'Full Day / Overnight',
        fee: 'Special Permits Required',
        image: 'https://images.unsplash.com/photo-1437622368342-7a3d73a34c8f?auto=format&fit=crop&w=800&q=80',
        desc: 'A world-class sea turtle conservation site where thousands of female turtles lay eggs annually.',
        reminders: 'Strictly requires DENR permits; restricted tourist entry to avoid disturbing wildlife.'
      }
    ],
    transport: 'Remote location requiring special sea transport coordination.',
    stayInfo: 'Government ranger station lodgings for authorized visits.'
  },
  {
    id: 'tandubas',
    name: 'Tandubas',
    tagline: 'Pristine lagoons and traditional mat-weaving artisans',
    municipality: 'Tandubas',
    type: 'Cultural & Eco-Destination',
    image: 'https://images.unsplash.com/photo-1507525428034-b723cf961d3e?auto=format&fit=crop&w=800&q=80',
    description: 'Famous for master weavers producing intricate Tepoh mats using natural pandan dyes, alongside crystal-clear lagoons.',
    highlights: ['Tepoh Mat Weaving', 'Ungus Matata Village', 'Coral Reef Reefs'],
    attractions: [
      {
        id: 'ungus-matata',
        name: 'Ungus Matata Weaving Village',
        type: 'Cultural Artisan Hub',
        duration: '2 hours',
        fee: 'Free',
        image: 'https://images.unsplash.com/photo-1606744824163-985d376605aa?auto=format&fit=crop&w=800&q=80',
        desc: 'Observe expert Sama women creating hand-woven floor mats with complex geometric patterns.',
        reminders: 'Consider buying mats directly from weavers to support local livelihoods.'
      }
    ],
    transport: 'Outrigger boat from main ports.',
    stayInfo: 'Community-based homestays.'
  },
  {
    id: 'mapun',
    name: 'Mapun',
    tagline: 'Remote volcanic lakes, lush foliage & Jama Mapun heritage',
    municipality: 'Mapun',
    type: 'Eco-Adventure & Remote Culture',
    image: 'https://images.unsplash.com/photo-1501785888041-af3ef285b470?auto=format&fit=crop&w=800&q=80',
    description: 'An isolated island municipality situated near the Sulu Sea border, featuring volcanic crater lakes and unique agricultural traditions.',
    highlights: ['Lake Sani', 'Duhul Batu Beach', 'Jama Mapun Cultural Heritage'],
    attractions: [
      {
        id: 'lake-sani',
        name: 'Lake Sani (Freshwater Crater Lake)',
        type: 'Geological Wonder',
        duration: '2 - 3 hours',
        fee: 'Free',
        image: 'https://images.unsplash.com/photo-1500382017468-9049fed747ef?auto=format&fit=crop&w=800&q=80',
        desc: 'A tranquil freshwater crater lake surrounded by lush coconut groves and endemic flora.',
        reminders: 'Requires local guide arrangements.'
      }
    ],
    transport: 'Long-haul passenger ferry from Zamboanga or Bongao.',
    stayInfo: 'Municipal guest rooms and local host families.'
  }
];

const ACCOMMODATIONS_DATA = [
  {
    id: 1,
    name: 'Rachel\'s Place Hotel & Restaurant',
    island: 'Bongao',
    type: 'Mid-Range Hotel',
    price: '₱1,800 - ₱3,200',
    contact: '+63 917 123 4567',
    rating: '4.7',
    features: ['Air Conditioning', 'Free Wi-Fi', 'On-site Restaurant', 'Airport Shuttle'],
    desc: 'One of Bongao\'s most established hotels offering air-conditioned rooms, hot showers, and delicious seafood options.',
    image: 'https://images.unsplash.com/photo-1566073771259-6a8506099945?auto=format&fit=crop&w=800&q=80'
  },
  {
    id: 2,
    name: 'Al-Barka Beach Resort',
    island: 'Bongao',
    type: 'Resort / Coastal',
    price: '₱2,200 - ₱4,000',
    contact: '+63 920 987 6543',
    rating: '4.5',
    features: ['Ocean View', 'Private Cottages', 'Breakfast Included', 'Tour Desk'],
    desc: 'Located along the coast with relaxing sea views and private bamboo huts ideal for winding down after island hopping.',
    image: 'https://images.unsplash.com/photo-1540541338287-41700207dee6?auto=format&fit=crop&w=800&q=80'
  },
  {
    id: 3,
    name: 'Bongao Prime Guesthouse',
    island: 'Bongao',
    type: 'Budget Inn',
    price: '₱800 - ₱1,500',
    contact: '+63 908 555 1234',
    rating: '4.2',
    features: ['Clean Rooms', 'Town Center Access', '24/7 Security'],
    desc: 'Budget-friendly accommodations right in the town center within walking distance to markets and boat piers.',
    image: 'https://images.unsplash.com/photo-1590490360182-c33d57733427?auto=format&fit=crop&w=800&q=80'
  },
  {
    id: 4,
    name: 'Simunul Heritage Homestay',
    island: 'Simunul',
    type: 'Community Homestay',
    price: '₱500 - ₱1,000',
    contact: 'Contact Municipal Tourism Office',
    rating: '4.6',
    features: ['Home-cooked Meals', 'Local Cultural Guide', 'Close to Mosque'],
    desc: 'Immerse yourself in authentic Sama culture staying directly with friendly local host families in Simunul.',
    image: 'https://images.unsplash.com/photo-1582719478250-c89cae4dc85b?auto=format&fit=crop&w=800&q=80'
  }
];

const CULTURES_DATA = [
  {
    id: 'sama',
    group: 'Sama Communities (Sama Dilaut / Sama Deya)',
    language: 'Sinama',
    heritage: 'Renowned for their peaceful maritime lifestyle, sea-dwelling traditions, and exquisite mat-weaving craft (Tepoh).',
    crafts: 'Tepoh (woven pandan mats), traditional boat carving, woodcraft.',
    food: 'Fresh roasted fish, Pyanggang, Utap, Kilawin seafood.',
    attire: 'Vibrant Malong and ceremonial Badju Lapi tops with brass buttons.',
    image: 'https://images.unsplash.com/photo-1544551763-46a013bb70d5?auto=format&fit=crop&w=800&q=80'
  },
  {
    id: 'tausug',
    group: 'Tausug Communities',
    language: 'Bahasa Sug',
    heritage: 'Known as "People of the Current," historically renowned for maritime bravery, trading routes, and rich culinary arts.',
    crafts: 'Pis Syabit (intricate geometric woven tapestry headscarves), weapon forging.',
    food: 'Satti with Taumu rice, Tiula Itum (black beef soup cooked with burnt coconut meat).',
    attire: 'Biyu-biyu blouses and tight-fitting trousers with metallic thread ornamentation.',
    image: 'https://images.unsplash.com/photo-1606744824163-985d376605aa?auto=format&fit=crop&w=800&q=80'
  },
  {
    id: 'jama-mapun',
    group: 'Jama Mapun',
    language: 'Pullun Mapun',
    heritage: 'Primarily inhabiting Mapun and Gazing islands, combining coconut agriculture with expert shallow-water navigation.',
    crafts: 'Unique basketry, coconut fiber crafts, and traditional herbal remedies.',
    food: 'Coconut-infused stews, cassava delicacies, and dried smoked fish.',
    attire: 'Handwoven cloth with distinct earth-tone geometric borders.',
    image: 'https://images.unsplash.com/photo-1501785888041-af3ef285b470?auto=format&fit=crop&w=800&q=80'
  }
];

const FOOD_DATA = [
  {
    name: 'Tiula Itum',
    type: 'Main Dish',
    desc: 'Rich, aromatic beef broth seasoned with burnt coconut paste, ginger, lemongrass, and native spices.',
    ingredients: 'Beef tenderloin, charred coconut meat, turmeric, galangal, chili.',
    image: 'https://images.unsplash.com/photo-1547592166-23ac45744acd?auto=format&fit=crop&w=800&q=80'
  },
  {
    name: 'Satti',
    type: 'Breakfast Specialty',
    desc: 'Grilled skewers of chicken or beef served alongside rice cubes (Taumu) submerged in a sweet-and-spicy thick red sauce.',
    ingredients: 'Chicken/Beef, chili, peanut base, annatto powder, rice cakes.',
    image: 'https://images.unsplash.com/photo-1555939594-58d7cb561ad1?auto=format&fit=crop&w=800&q=80'
  },
  {
    name: 'Fresh Tawi-Tawi Seafood',
    type: 'Coastal Feast',
    desc: 'Giant curacha crabs, lobsters, sea urchins (Uni), and grilled lapu-lapu prepared fresh directly from municipal markets.',
    ingredients: 'Fresh marine catch, butter, garlic, sea salt.',
    image: 'https://images.unsplash.com/photo-1534422298391-e4f8c172dddb?auto=format&fit=crop&w=800&q=80'
  },
  {
    name: 'Jangay / Tawi Snacks',
    type: 'Traditional Pastry',
    desc: 'Deep-fried cassava crisps, rice flour rosettes, and sweet fried banana snacks enjoyed during afternoon tea.',
    ingredients: 'Cassava, palm sugar, coconut oil.',
    image: 'https://images.unsplash.com/photo-1578985545062-69928b1d9587?auto=format&fit=crop&w=800&q=80'
  }
];

// --- MAIN APP COMPONENT ---

export default function App() {
  const [activeTab, setActiveTab] = useState('home');
  const [searchQuery, setSearchQuery] = useState('');
  const [selectedIsland, setSelectedIsland] = useState(null);
  const [favorites, setFavorites] = useState([]);
  const [selectedModalAttraction, setSelectedModalAttraction] = useState(null);

  // Adventure Finder Filters
  const [selectedInterests, setSelectedInterests] = useState([]);

  // Budget Calculator State
  const [calcTravelers, setCalcTravelers] = useState(2);
  const [calcDays, setCalcDays] = useState(4);
  const [calcLodging, setCalcLodging] = useState('mid'); // budget, mid, luxury
  const [calcBoats, setCalcBoats] = useState('shared'); // shared, private

  // Itinerary Builder State
  const [itinDays, setItinDays] = useState('4-5');
  const [itinPace, setItinPace] = useState('balanced');

  // Toggle Favorites
  const toggleFavorite = (itemId) => {
    if (favorites.includes(itemId)) {
      setFavorites(favorites.filter(id => id !== itemId));
    } else {
      setFavorites([...favorites, itemId]);
    }
  };

  // Calculated Budget Total
  const calculatedBudget = useMemo(() => {
    let lodgingPerNight = calcLodging === 'budget' ? 1000 : calcLodging === 'mid' ? 2500 : 4500;
    let foodPerDayPerPerson = 600;
    let boatPerDay = calcBoats === 'shared' ? 800 : 3500;
    let miscPerPerson = 300;

    const totalLodging = lodgingPerNight * (calcDays - 1);
    const totalFood = foodPerDayPerPerson * calcDays * calcTravelers;
    const totalBoats = boatPerDay * calcDays;
    const totalMisc = miscPerPerson * calcDays * calcTravelers;

    const grandTotal = totalLodging + totalFood + totalBoats + totalMisc;
    return {
      lodging: totalLodging,
      food: totalFood,
      boats: totalBoats,
      misc: totalMisc,
      grandTotal,
      perPerson: Math.round(grandTotal / calcTravelers)
    };
  }, [calcTravelers, calcDays, calcLodging, calcBoats]);

  // Filtered Islands for Exploration
  const filteredIslands = useMemo(() => {
    return ISLANDS_DATA.filter(island => {
      const matchesSearch = island.name.toLowerCase().includes(searchQuery.toLowerCase()) ||
                            island.tagline.toLowerCase().includes(searchQuery.toLowerCase()) ||
                            island.highlights.some(h => h.toLowerCase().includes(searchQuery.toLowerCase()));
      return matchesSearch;
    });
  }, [searchQuery]);

  return (
    <div className="min-h-screen bg-[#F4F1EA] text-[#0B2545] font-sans antialiased flex flex-col selection:bg-[#139A8C] selection:text-white">
      
      {/* --- TOP NAVIGATION BAR --- */}
      <header className="sticky top-0 z-40 bg-[#0B2545] text-white shadow-lg border-b border-[#139A8C]/30">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div className="flex items-center justify-between h-20">
            
            {/* LOGO */}
            <div 
              onClick={() => setActiveTab('home')} 
              className="cursor-pointer flex items-center space-x-3 group"
            >
              <div className="w-10 h-10 rounded-full bg-gradient-to-tr from-[#139A8C] to-[#EE9B00] flex items-center justify-center text-white shadow-md group-hover:scale-105 transition-transform">
                <Anchor className="w-5 h-5" />
              </div>
              <div>
                <h1 className="text-xl font-bold tracking-tight font-serif text-white group-hover:text-[#EE9B00] transition-colors">
                  TAWI-TAWI
                </h1>
                <p className="text-[10px] tracking-widest text-[#139A8C] uppercase font-semibold">
                  Island Province Travel Guide
                </p>
              </div>
            </div>

            {/* DESKTOP NAV ITEMS */}
            <nav className="hidden lg:flex items-center space-x-1 text-sm font-medium">
              {[
                { id: 'home', label: 'Home' },
                { id: 'islands', label: 'Explore Islands' },
                { id: 'map', label: 'Interactive Map' },
                { id: 'planner', label: 'Itinerary & Quiz' },
                { id: 'budget', label: 'Budget Calculator' },
                { id: 'stay', label: 'Where to Stay' },
                { id: 'culture', label: 'People & Culture' },
                { id: 'kamardikaan', label: 'Kamardikaan' },
                { id: 'food', label: 'Food & Crafts' },
                { id: 'guide', label: 'Before You Visit' },
              ].map((tab) => (
                <button
                  key={tab.id}
                  onClick={() => setActiveTab(tab.id)}
                  className={`px-3 py-2 rounded-md transition-all duration-200 ${
                    activeTab === tab.id
                      ? 'bg-[#139A8C] text-white font-semibold shadow-sm'
                      : 'text-slate-200 hover:bg-[#0B2545]/60 hover:text-[#EE9B00]'
                  }`}
                >
                  {tab.label}
                </button>
              ))}
            </nav>

            {/* ACTION BUTTON & FAVORITES BADGE */}
            <div className="flex items-center space-x-3">
              <button 
                onClick={() => setActiveTab('planner')}
                className="hidden sm:flex items-center space-x-2 bg-gradient-to-r from-[#EE9B00] to-[#CA6702] text-white font-semibold px-4 py-2 rounded-full hover:shadow-lg hover:brightness-110 transition-all transform active:scale-95 text-xs uppercase tracking-wider"
              >
                <Sparkles className="w-4 h-4" />
                <span>Plan Your Visit</span>
              </button>

              <button 
                onClick={() => setActiveTab('islands')}
                className="relative p-2 text-slate-200 hover:text-[#EE9B00] transition-colors"
                title="Saved Items"
              >
                <Heart className="w-6 h-6" />
                {favorites.length > 0 && (
                  <span className="absolute top-0 right-0 inline-flex items-center justify-center px-1.5 py-0.5 text-xs font-bold leading-none text-white bg-red-500 rounded-full">
                    {favorites.length}
                  </span>
                )}
              </button>
            </div>
          </div>
        </div>

        {/* MOBILE NAVIGATION SCROLL BAR */}
        <div className="lg:hidden border-t border-slate-700/50 bg-[#0B2545]/95 overflow-x-auto scrollbar-none py-2 px-4 flex space-x-2">
          {[
            { id: 'home', label: 'Home' },
            { id: 'islands', label: 'Islands' },
            { id: 'map', label: 'Map' },
            { id: 'planner', label: 'Itinerary' },
            { id: 'budget', label: 'Budget' },
            { id: 'stay', label: 'Stay' },
            { id: 'culture', label: 'Culture' },
            { id: 'kamardikaan', label: 'Kamardikaan' },
            { id: 'food', label: 'Food' },
            { id: 'guide', label: 'Travel Guide' },
          ].map((tab) => (
            <button
              key={tab.id}
              onClick={() => setActiveTab(tab.id)}
              className={`whitespace-nowrap px-3 py-1.5 text-xs rounded-full font-medium transition-colors ${
                activeTab === tab.id
                  ? 'bg-[#139A8C] text-white'
                  : 'bg-slate-800/80 text-slate-300 hover:bg-slate-700'
              }`}
            >
              {tab.label}
            </button>
          ))}
        </div>
      </header>

      {/* --- SEARCH BAR STRIP --- */}
      <div className="bg-[#0B2545]/90 border-b border-[#139A8C]/20 py-3 px-4 shadow-inner">
        <div className="max-w-4xl mx-auto flex items-center bg-white/10 backdrop-blur-md rounded-full px-4 py-2 border border-white/20 text-white shadow-sm focus-within:ring-2 focus-within:ring-[#139A8C]">
          <Search className="w-5 h-5 text-slate-300 mr-2 shrink-0" />
          <input
            type="text"
            placeholder="Search islands, sacred mountains, stilt markets, dishes, mosques..."
            value={searchQuery}
            onChange={(e) => setSearchQuery(e.target.value)}
            className="w-full bg-transparent border-none outline-none text-sm placeholder-slate-300 text-white"
          />
          {searchQuery && (
            <button onClick={() => setSearchQuery('')} className="text-slate-300 hover:text-white text-xs">
              <X className="w-4 h-4" />
            </button>
          )}
        </div>
      </div>

      {/* --- DYNAMIC MAIN CONTENT SECTIONS --- */}
      <main className="flex-grow">
        
        {/* ================= 1. HOME / LANDING ================= */}
        {activeTab === 'home' && (
          <div className="space-y-16 pb-16">
            
            {/* HERO SECTION */}
            <div className="relative min-h-[85vh] flex items-center justify-center text-center px-4 overflow-hidden bg-slate-900">
              <div 
                className="absolute inset-0 bg-cover bg-center bg-no-repeat opacity-60 transform scale-105 transition-transform duration-10000 hover:scale-100"
                style={{ backgroundImage: `url('https://images.unsplash.com/photo-1507525428034-b723cf961d3e?auto=format&fit=crop&w=2000&q=80')` }}
              ></div>
              <div className="absolute inset-0 bg-gradient-to-t from-[#0B2545] via-[#0B2545]/40 to-transparent"></div>
              
              <div className="relative z-10 max-w-4xl mx-auto space-y-6 pt-12">
                <div className="inline-flex items-center space-x-2 px-4 py-1.5 rounded-full bg-[#139A8C]/80 backdrop-blur-md border border-white/30 text-white text-xs font-semibold tracking-widest uppercase">
                  <Sun className="w-4 h-4 text-[#EE9B00]" />
                  <span>The Southernmost Frontier of the Philippines</span>
                </div>
                
                <h1 className="text-4xl sm:text-6xl md:text-7xl font-serif font-extrabold text-white leading-tight drop-shadow-md">
                  TAWI-TAWI ISLANDS
                </h1>
                <p className="text-lg sm:text-2xl text-slate-200 font-light max-w-3xl mx-auto leading-relaxed">
                  Discover the sacred peaks, crystal waters, living maritime culture, and rich heritage before you visit.
                </p>

                <div className="pt-6 flex flex-wrap justify-center gap-4">
                  <button 
                    onClick={() => setActiveTab('islands')}
                    className="flex items-center space-x-2 bg-[#EE9B00] hover:bg-[#CA6702] text-white font-bold px-8 py-4 rounded-full shadow-2xl transition-all transform hover:-translate-y-0.5 text-sm uppercase tracking-wider"
                  >
                    <span>Explore Island Directory</span>
                    <ArrowRight className="w-5 h-5" />
                  </button>
                  <button 
                    onClick={() => setActiveTab('planner')}
                    className="flex items-center space-x-2 bg-white/20 hover:bg-white/30 backdrop-blur-md text-white border border-white/40 font-semibold px-8 py-4 rounded-full transition-all text-sm uppercase tracking-wider"
                  >
                    <Sparkles className="w-5 h-5 text-[#EE9B00]" />
                    <span>Take Adventure Quiz</span>
                  </button>
                </div>
              </div>
            </div>

            {/* QUICK CATEGORY HIGHLIGHTS */}
            <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
              <div className="text-center space-y-3 mb-12">
                <h2 className="text-3xl font-serif font-bold text-[#0B2545]">The Treasures of Tawi-Tawi</h2>
                <p className="text-slate-600 max-w-2xl mx-auto text-sm">
                  From holy mountain treks to ancient stilt communities and turtle reserves, explore what makes this province unique.
                </p>
              </div>

              <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
                {[
                  {
                    title: 'Sacred Heights',
                    desc: 'Bud Bongao peak, revered pilgrimages, and lush fauna.',
                    icon: Navigation,
                    tab: 'islands',
                    img: 'https://images.unsplash.com/photo-1464822759023-fed622ff2c3b?auto=format&fit=crop&w=600&q=80'
                  },
                  {
                    title: 'Cradle of Faith',
                    desc: 'Sheik Karimul Makhdum Mosque in Simunul built in 1380 AD.',
                    icon: BookOpen,
                    tab: 'islands',
                    img: 'https://images.unsplash.com/photo-1584551246679-0daf3d275d0f?auto=format&fit=crop&w=600&q=80'
                  },
                  {
                    title: 'Aquatic Living',
                    desc: 'Sitangkai stilt town with canals replacing roads.',
                    icon: Anchor,
                    tab: 'islands',
                    img: 'https://images.unsplash.com/photo-1518509562904-e7ef99cdcc86?auto=format&fit=crop&w=600&q=80'
                  },
                  {
                    title: 'Turtle Sanctuaries',
                    desc: 'Baguan Island marine conservation and turtle nesting sites.',
                    icon: ShieldAlert,
                    tab: 'islands',
                    img: 'https://images.unsplash.com/photo-1437622368342-7a3d73a34c8f?auto=format&fit=crop&w=600&q=80'
                  }
                ].map((card, index) => (
                  <div 
                    key={index}
                    onClick={() => setActiveTab(card.tab)}
                    className="group relative rounded-2xl overflow-hidden shadow-lg cursor-pointer h-80 transition-all duration-300 hover:-translate-y-1.5 hover:shadow-2xl"
                  >
                    <img src={card.img} alt={card.title} className="absolute inset-0 w-full h-full object-cover group-hover:scale-110 transition-transform duration-700" />
                    <div className="absolute inset-0 bg-gradient-to-t from-[#0B2545] via-[#0B2545]/40 to-transparent"></div>
                    <div className="absolute bottom-0 p-6 text-white space-y-2">
                      <card.icon className="w-8 h-8 text-[#EE9B00] mb-2" />
                      <h3 className="text-xl font-serif font-bold group-hover:text-[#EE9B00] transition-colors">{card.title}</h3>
                      <p className="text-xs text-slate-300 leading-relaxed">{card.desc}</p>
                    </div>
                  </div>
                ))}
              </div>
            </div>

            {/* FEATURED DESTINATION SPOTLIGHT: BONGAO */}
            <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
              <div className="bg-gradient-to-br from-[#0B2545] to-[#139A8C] rounded-3xl p-8 lg:p-12 text-white shadow-2xl relative overflow-hidden">
                <div className="grid grid-cols-1 lg:grid-cols-2 gap-8 items-center relative z-10">
                  <div className="space-y-6">
                    <span className="bg-[#EE9B00] text-slate-900 font-bold px-3 py-1 rounded-full text-xs uppercase tracking-wider">
                      Gateway Spotlight
                    </span>
                    <h2 className="text-3xl sm:text-5xl font-serif font-bold leading-tight">
                      Bongao: The Heartbeat of Tawi-Tawi
                    </h2>
                    <p className="text-slate-200 text-sm sm:text-base leading-relaxed">
                      Bongao is your welcoming landing pad in Tawi-Tawi. Home to sacred Bud Bongao mountain, the ancient archaeology of Boloboc Cave, the coastal beauty of Bihing Tahik, and the vibrant waterborne life at Chinese Pier.
                    </p>
                    <div className="flex flex-wrap gap-3 pt-2">
                      <span className="bg-white/10 px-3 py-1 rounded-lg text-xs font-medium border border-white/20">Bud Bongao Peak</span>
                      <span className="bg-white/10 px-3 py-1 rounded-lg text-xs font-medium border border-white/20">Bihing Tahik Beach</span>
                      <span className="bg-white/10 px-3 py-1 rounded-lg text-xs font-medium border border-white/20">Boloboc Cave</span>
                      <span className="bg-white/10 px-3 py-1 rounded-lg text-xs font-medium border border-white/20">Chinese Pier</span>
                    </div>
                    <button 
                      onClick={() => {
                        setSelectedIsland(ISLANDS_DATA.find(i => i.id === 'bongao'));
                        setActiveTab('islands');
                      }}
                      className="inline-flex items-center space-x-2 bg-white text-[#0B2545] font-bold px-6 py-3 rounded-full hover:bg-[#EE9B00] hover:text-white transition-all shadow-md text-sm"
                    >
                      <span>Read Full Bongao Guide</span>
                      <ChevronRight className="w-4 h-4" />
                    </button>
                  </div>
                  <div className="grid grid-cols-2 gap-4">
                    <img src="https://images.unsplash.com/photo-1464822759023-fed622ff2c3b?auto=format&fit=crop&w=600&q=80" alt="Bud Bongao" className="rounded-2xl shadow-md h-48 w-full object-cover hover:scale-105 transition-transform" />
                    <img src="https://images.unsplash.com/photo-1519046904884-53103b34b206?auto=format&fit=crop&w=600&q=80" alt="Bihing Tahik" className="rounded-2xl shadow-md h-48 w-full object-cover hover:scale-105 transition-transform" />
                    <img src="https://images.unsplash.com/photo-1509316975850-ff9c5deb0cd9?auto=format&fit=crop&w=600&q=80" alt="Boloboc Cave" className="rounded-2xl shadow-md h-48 w-full object-cover hover:scale-105 transition-transform" />
                    <img src="https://images.unsplash.com/photo-1544551763-46a013bb70d5?auto=format&fit=crop&w=600&q=80" alt="Chinese Pier" className="rounded-2xl shadow-md h-48 w-full object-cover hover:scale-105 transition-transform" />
                  </div>
                </div>
              </div>
            </div>

            {/* ETHICAL TOURISM BANNER */}
            <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
              <div className="bg-[#EEF4F8] border border-[#139A8C]/30 rounded-2xl p-6 sm:p-8 flex flex-col md:flex-row items-center justify-between gap-6">
                <div className="flex items-start space-x-4">
                  <div className="p-3 bg-[#139A8C] text-white rounded-xl shrink-0">
                    <ShieldAlert className="w-8 h-8" />
                  </div>
                  <div>
                    <h3 className="text-xl font-serif font-bold text-[#0B2545]">Responsible Tourism Pledge</h3>
                    <p className="text-slate-600 text-sm mt-1 max-w-2xl">
                      Tawi-Tawi is a rich ecological and cultural domain. Respect religious traditions at mosques, ask permission before taking portraits, preserve coral reefs, and avoid littering on sacred sites.
                    </p>
                  </div>
                </div>
                <button 
                  onClick={() => setActiveTab('guide')}
                  className="whitespace-nowrap px-6 py-3 bg-[#0B2545] text-white font-semibold rounded-full text-xs uppercase tracking-wider hover:bg-[#139A8C] transition-colors"
                >
                  View Travel Etiquette
                </button>
              </div>
            </div>

          </div>
        )}

        {/* ================= 2. EXPLORE ISLANDS DIRECTORY ================= */}
        {activeTab === 'islands' && (
          <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10 space-y-8">
            
            <div className="flex flex-col md:flex-row md:items-end justify-between gap-4 border-b border-slate-300 pb-6">
              <div>
                <span className="text-xs font-bold text-[#139A8C] uppercase tracking-widest">Island Destination Directory</span>
                <h2 className="text-3xl sm:text-4xl font-serif font-bold text-[#0B2545] mt-1">Explore Tawi-Tawi Islands</h2>
                <p className="text-slate-600 text-sm max-w-2xl mt-1">
                  Click on any island below for comprehensive travel information, verified tourist spots, local transportation tips, and visitor guidelines.
                </p>
              </div>

              {selectedIsland && (
                <button 
                  onClick={() => setSelectedIsland(null)}
                  className="self-start md:self-auto text-xs font-semibold text-[#139A8C] hover:text-[#0B2545] bg-[#EEF4F8] px-4 py-2 rounded-full border border-[#139A8C]/30 flex items-center space-x-2"
                >
                  <X className="w-4 h-4" />
                  <span>Show All Islands</span>
                </button>
              )}
            </div>

            {/* IF AN ISLAND IS SELECTED FOR DETAILED VIEW */}
            {selectedIsland ? (
              <div className="bg-white rounded-3xl shadow-xl overflow-hidden border border-slate-200 animate-fadeIn">
                <div className="relative h-80 sm:h-96">
                  <img src={selectedIsland.image} alt={selectedIsland.name} className="w-full h-full object-cover" />
                  <div className="absolute inset-0 bg-gradient-to-t from-[#0B2545] via-transparent to-transparent"></div>
                  <div className="absolute bottom-6 left-6 right-6 text-white flex flex-col md:flex-row md:items-end justify-between gap-4">
                    <div>
                      <span className="bg-[#EE9B00] text-slate-900 font-bold px-3 py-1 rounded-full text-xs uppercase tracking-wider">
                        {selectedIsland.municipality} Municipality
                      </span>
                      <h3 className="text-4xl font-serif font-bold mt-2">{selectedIsland.name} Island</h3>
                      <p className="text-slate-200 text-sm mt-1">{selectedIsland.tagline}</p>
                    </div>

                    <button 
                      onClick={() => toggleFavorite(selectedIsland.id)}
                      className={`px-4 py-2 rounded-full text-xs font-bold flex items-center space-x-2 border transition-colors ${
                        favorites.includes(selectedIsland.id)
                          ? 'bg-red-500 text-white border-red-500'
                          : 'bg-white/20 backdrop-blur-md text-white border-white/40 hover:bg-white/40'
                      }`}
                    >
                      <Heart className={`w-4 h-4 ${favorites.includes(selectedIsland.id) ? 'fill-current' : ''}`} />
                      <span>{favorites.includes(selectedIsland.id) ? 'Saved to Favorites' : 'Save Island'}</span>
                    </button>
                  </div>
                </div>

                <div className="p-6 sm:p-10 space-y-10">
                  {/* OVERVIEW & LOGISTICS */}
                  <div className="grid grid-cols-1 lg:grid-cols-3 gap-8">
                    <div className="lg:col-span-2 space-y-4">
                      <h4 className="text-2xl font-serif font-bold text-[#0B2545]">About {selectedIsland.name}</h4>
                      <p className="text-slate-700 leading-relaxed text-sm sm:text-base">
                        {selectedIsland.description}
                      </p>
                      
                      <div className="pt-4">
                        <h5 className="text-sm font-bold text-[#0B2545] uppercase tracking-wider mb-2">Key Highlights</h5>
                        <div className="flex flex-wrap gap-2">
                          {selectedIsland.highlights.map((h, idx) => (
                            <span key={idx} className="bg-[#EEF4F8] text-[#0B2545] px-3 py-1 rounded-lg text-xs font-medium border border-[#139A8C]/20">
                              {h}
                            </span>
                          ))}
                        </div>
                      </div>
                    </div>

                    <div className="bg-[#EEF4F8] p-6 rounded-2xl border border-[#139A8C]/20 space-y-4">
                      <h4 className="text-lg font-serif font-bold text-[#0B2545]">Travel Info & Logistics</h4>
                      <div className="space-y-3 text-xs text-slate-700">
                        <div>
                          <strong className="block text-[#0B2545] font-semibold">How to reach:</strong>
                          <p>{selectedIsland.transport}</p>
                        </div>
                        <div>
                          <strong className="block text-[#0B2545] font-semibold">Accommodation:</strong>
                          <p>{selectedIsland.stayInfo}</p>
                        </div>
                        <div>
                          <strong className="block text-[#0B2545] font-semibold">Island Category:</strong>
                          <p>{selectedIsland.type}</p>
                        </div>
                      </div>
                    </div>
                  </div>

                  {/* ATTRACTIONS SECTION */}
                  <div className="space-y-6 pt-6 border-t border-slate-200">
                    <div className="flex items-center justify-between">
                      <h4 className="text-2xl font-serif font-bold text-[#0B2545]">Verified Tourist Spots ({selectedIsland.attractions.length})</h4>
                    </div>

                    <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
                      {selectedIsland.attractions.map((att) => (
                        <div 
                          key={att.id}
                          className="bg-white rounded-2xl border border-slate-200 overflow-hidden shadow-md hover:shadow-xl transition-shadow flex flex-col"
                        >
                          <div className="relative h-48">
                            <img src={att.image} alt={att.name} className="w-full h-full object-cover" />
                            <span className="absolute top-3 left-3 bg-[#0B2545]/80 backdrop-blur-md text-white text-[10px] font-bold px-2.5 py-1 rounded-full uppercase">
                              {att.type}
                            </span>
                          </div>
                          <div className="p-5 flex-grow flex flex-col justify-between space-y-4">
                            <div>
                              <h5 className="text-lg font-serif font-bold text-[#0B2545]">{att.name}</h5>
                              <p className="text-xs text-slate-600 mt-2 leading-relaxed">{att.desc}</p>
                            </div>

                            <div className="space-y-2 pt-2 border-t border-slate-100 text-xs">
                              <div className="flex justify-between text-slate-600">
                                <span><strong>Duration:</strong> {att.duration}</span>
                                <span><strong>Fee:</strong> {att.fee}</span>
                              </div>
                              <div className="bg-[#F4F1EA] p-2 rounded-lg text-[11px] text-slate-700">
                                <strong className="text-[#CA6702]">Visitor Reminder:</strong> {att.reminders}
                              </div>
                            </div>
                          </div>
                        </div>
                      ))}
                    </div>
                  </div>

                </div>
              </div>
            ) : (
              /* GRID OF ALL ISLANDS */
              <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
                {filteredIslands.map((island) => (
                  <div 
                    key={island.id}
                    className="bg-white rounded-2xl overflow-hidden shadow-md hover:shadow-2xl border border-slate-200 transition-all duration-300 flex flex-col group"
                  >
                    <div className="relative h-52 overflow-hidden">
                      <img 
                        src={island.image} 
                        alt={island.name} 
                        className="w-full h-full object-cover group-hover:scale-105 transition-transform duration-500" 
                      />
                      <div className="absolute inset-0 bg-gradient-to-t from-[#0B2545] via-transparent to-transparent"></div>
                      <span className="absolute top-3 left-3 bg-[#139A8C] text-white text-[10px] font-bold px-2.5 py-1 rounded-full uppercase tracking-wider">
                        {island.municipality}
                      </span>
                      <button 
                        onClick={() => toggleFavorite(island.id)}
                        className="absolute top-3 right-3 p-2 rounded-full bg-black/40 text-white hover:text-red-400 backdrop-blur-md"
                      >
                        <Heart className={`w-4 h-4 ${favorites.includes(island.id) ? 'fill-red-500 text-red-500' : ''}`} />
                      </button>
                      <div className="absolute bottom-3 left-4 text-white">
                        <h3 className="text-2xl font-serif font-bold">{island.name}</h3>
                      </div>
                    </div>

                    <div className="p-6 flex-grow flex flex-col justify-between space-y-4">
                      <div>
                        <p className="text-xs font-semibold text-[#139A8C] uppercase tracking-wider">{island.tagline}</p>
                        <p className="text-xs text-slate-600 mt-2 line-clamp-3 leading-relaxed">
                          {island.description}
                        </p>
                      </div>

                      <div className="space-y-3 pt-3 border-t border-slate-100">
                        <div className="flex flex-wrap gap-1">
                          {island.highlights.slice(0, 3).map((h, i) => (
                            <span key={i} className="text-[10px] bg-[#EEF4F8] text-[#0B2545] px-2 py-0.5 rounded font-medium">
                              {h}
                            </span>
                          ))}
                        </div>

                        <button 
                          onClick={() => setSelectedIsland(island)}
                          className="w-full py-2.5 bg-[#0B2545] hover:bg-[#139A8C] text-white font-semibold rounded-xl text-xs uppercase tracking-wider transition-colors flex items-center justify-center space-x-1"
                        >
                          <span>View Full Island Guide</span>
                          <ChevronRight className="w-4 h-4" />
                        </button>
                      </div>
                    </div>
                  </div>
                ))}
              </div>
            )}

          </div>
        )}

        {/* ================= 3. INTERACTIVE MAP & PLANNER ================= */}
        {activeTab === 'map' && (
          <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10 space-y-8">
            <div className="border-b border-slate-300 pb-4">
              <span className="text-xs font-bold text-[#139A8C] uppercase tracking-widest">Visual Map Explorer</span>
              <h2 className="text-3xl font-serif font-bold text-[#0B2545] mt-1">Interactive Tawi-Tawi Archipelago Map</h2>
              <p className="text-slate-600 text-sm mt-1">
                Select an island hub on the visual map below to inspect location data, key attractions, and transport access.
              </p>
            </div>

            <div className="grid grid-cols-1 lg:grid-cols-3 gap-8">
              {/* MAP VISUAL AREA */}
              <div className="lg:col-span-2 bg-[#0B2545] rounded-3xl p-6 text-white relative overflow-hidden min-h-[450px] flex flex-col justify-between border border-[#139A8C]/30 shadow-2xl">
                
                {/* WATER BACKGROUND PATTERN */}
                <div className="absolute inset-0 opacity-10 bg-[radial-gradient(#139A8C_1px,transparent_1px)] [background-size:16px_16px]"></div>
                
                <div className="relative z-10 flex justify-between items-center">
                  <div className="flex items-center space-x-2 bg-white/10 backdrop-blur-md px-3 py-1.5 rounded-full border border-white/20 text-xs">
                    <Compass className="w-4 h-4 text-[#EE9B00]" />
                    <span>Sulu Sea / Celebes Sea Region</span>
                  </div>
                  <span className="text-xs text-slate-400">Click a island pin to load guide</span>
                </div>

                {/* SVG ARCHIPELAGO REPRESENTATION WITH CLICKABLE PINS */}
                <div className="relative z-10 my-8 flex items-center justify-center">
                  <svg viewBox="0 0 800 500" className="w-full h-auto max-h-[350px]">
                    {/* WATER WAVES */}
                    <path d="M 50 100 Q 150 120 250 100 T 450 100 T 650 100" fill="none" stroke="#139A8C" strokeWidth="0.5" opacity="0.3" />
                    <path d="M 100 250 Q 200 270 300 250 T 500 250 T 700 250" fill="none" stroke="#139A8C" strokeWidth="0.5" opacity="0.3" />
                    
                    {/* MAP ISLAND HUB PINS */}
                    {[
                      { id: 'turtle-islands', name: 'Turtle Islands', x: 200, y: 80, color: '#EE9B00' },
                      { id: 'mapun', name: 'Mapun Island', x: 580, y: 90, color: '#139A8C' },
                      { id: 'bongao', name: 'Bongao (Capital Hub)', x: 400, y: 260, color: '#EE9B00' },
                      { id: 'sanga-sanga', name: 'Sanga-Sanga Airport', x: 440, y: 240, color: '#139A8C' },
                      { id: 'simunul', name: 'Simunul (1380 Mosque)', x: 420, y: 310, color: '#139A8C' },
                      { id: 'tandubas', name: 'Tandubas', x: 530, y: 230, color: '#139A8C' },
                      { id: 'sibutu', name: 'Sibutu Island', x: 320, y: 370, color: '#139A8C' },
                      { id: 'sitangkai', name: 'Sitangkai (Venice)', x: 260, y: 430, color: '#EE9B00' }
                    ].map((pin) => (
                      <g 
                        key={pin.id} 
                        onClick={() => setSelectedIsland(ISLANDS_DATA.find(i => i.id === pin.id))}
                        className="cursor-pointer group"
                      >
                        <circle cx={pin.x} cy={pin.y} r="18" fill={pin.color} opacity="0.2" className="animate-ping" />
                        <circle cx={pin.x} cy={pin.y} r="10" fill={pin.color} className="group-hover:scale-125 transition-transform" />
                        <circle cx={pin.x} cy={pin.y} r="4" fill="#FFFFFF" />
                        <text x={pin.x + 15} y={pin.y + 4} fill="#FFFFFF" fontSize="12" fontWeight="bold" className="font-sans group-hover:fill-[#EE9B00]">
                          {pin.name}
                        </text>
                      </g>
                    ))}
                  </svg>
                </div>

                <div className="relative z-10 flex flex-wrap gap-4 text-xs text-slate-300 bg-black/30 p-3 rounded-xl border border-white/10">
                  <span className="flex items-center"><span className="w-2.5 h-2.5 rounded-full bg-[#EE9B00] mr-1.5"></span> Major Gateway / Key Hub</span>
                  <span className="flex items-center"><span className="w-2.5 h-2.5 rounded-full bg-[#139A8C] mr-1.5"></span> Cultural & Ecological Destination</span>
                </div>
              </div>

              {/* MAP SIDE PANEL DETAILS */}
              <div className="bg-white rounded-3xl p-6 border border-slate-200 shadow-lg space-y-6 flex flex-col justify-between">
                <div>
                  <h3 className="text-xl font-serif font-bold text-[#0B2545]">
                    {selectedIsland ? selectedIsland.name : 'Select an Island on Map'}
                  </h3>
                  <p className="text-xs text-slate-500 mt-1">
                    {selectedIsland ? selectedIsland.tagline : 'Click any glowing pin on the map to preview destination details.'}
                  </p>

                  {selectedIsland ? (
                    <div className="mt-6 space-y-4 text-xs text-slate-700">
                      <img src={selectedIsland.image} alt={selectedIsland.name} className="w-full h-40 object-cover rounded-2xl shadow-sm" />
                      <div>
                        <strong className="text-[#0B2545]">Municipality:</strong> {selectedIsland.municipality}
                      </div>
                      <div>
                        <strong className="text-[#0B2545]">Description:</strong>
                        <p className="mt-1 leading-relaxed">{selectedIsland.description}</p>
                      </div>
                      <div>
                        <strong className="text-[#0B2545]">Top Attractions:</strong>
                        <ul className="list-disc list-inside mt-1 space-y-1 text-slate-600">
                          {selectedIsland.highlights.map((h, i) => <li key={i}>{h}</li>)}
                        </ul>
                      </div>
                    </div>
                  ) : (
                    <div className="mt-12 text-center text-slate-400 space-y-3">
                      <MapPin className="w-12 h-12 mx-auto text-slate-300" />
                      <p className="text-xs">No island selected. Pick a point on the map to inspect key travel details.</p>
                    </div>
                  )}
                </div>

                {selectedIsland && (
                  <button 
                    onClick={() => setActiveTab('islands')}
                    className="w-full py-3 bg-[#139A8C] hover:bg-[#0B2545] text-white font-bold rounded-xl text-xs uppercase tracking-wider transition-colors"
                  >
                    Open Full {selectedIsland.name} Directory
                  </button>
                )}
              </div>
            </div>
          </div>
        )}

        {/* ================= 4. ITINERARY BUILDER & ADVENTURE QUIZ ================= */}
        {activeTab === 'planner' && (
          <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10 space-y-12">
            
            {/* ADVENTURE FINDER QUIZ */}
            <div className="bg-white rounded-3xl p-8 border border-slate-200 shadow-xl space-y-6">
              <div className="border-b border-slate-200 pb-4">
                <span className="text-xs font-bold text-[#EE9B00] uppercase tracking-widest">Interactive Quiz</span>
                <h2 className="text-3xl font-serif font-bold text-[#0B2545] mt-1">Choose Your Island Adventure</h2>
                <p className="text-slate-600 text-sm mt-1">
                  Select your travel preferences below to discover recommendations tailored for you.
                </p>
              </div>

              <div className="flex flex-wrap gap-3">
                {[
                  { id: 'hiking', label: 'Sacred Mountains & Hiking' },
                  { id: 'heritage', label: '1380 Islamic Heritage' },
                  { id: 'water', label: 'Stilt Houses & Water Canals' },
                  { id: 'turtles', label: 'Wildlife & Marine Turtles' },
                  { id: 'crafts', label: 'Boat Building & Weaving' },
                  { id: 'seafood', label: 'Fresh Seafood & Markets' }
                ].map((item) => {
                  const isSelected = selectedInterests.includes(item.id);
                  return (
                    <button
                      key={item.id}
                      onClick={() => {
                        if (isSelected) {
                          setSelectedInterests(selectedInterests.filter(i => i !== item.id));
                        } else {
                          setSelectedInterests([...selectedInterests, item.id]);
                        }
                      }}
                      className={`px-4 py-2.5 rounded-full text-xs font-bold transition-all border ${
                        isSelected
                          ? 'bg-[#139A8C] text-white border-[#139A8C] shadow-md'
                          : 'bg-[#EEF4F8] text-[#0B2545] border-slate-200 hover:bg-slate-200'
                      }`}
                    >
                      {item.label}
                    </button>
                  );
                })}
              </div>

              <div className="bg-[#EEF4F8] p-6 rounded-2xl border border-[#139A8C]/20">
                <h4 className="text-sm font-bold text-[#0B2545] uppercase tracking-wider mb-2">
                  Recommended Destinations ({selectedInterests.length === 0 ? 'All Islands' : 'Filtered Matches'}):
                </h4>
                <div className="grid grid-cols-1 sm:grid-cols-3 gap-4">
                  {ISLANDS_DATA.filter(island => {
                    if (selectedInterests.length === 0) return true;
                    if (selectedInterests.includes('hiking') && island.id === 'bongao') return true;
                    if (selectedInterests.includes('heritage') && island.id === 'simunul') return true;
                    if (selectedInterests.includes('water') && island.id === 'sitangkai') return true;
                    if (selectedInterests.includes('turtles') && island.id === 'turtle-islands') return true;
                    if (selectedInterests.includes('crafts') && (island.id === 'sibutu' || island.id === 'tandubas')) return true;
                    return false;
                  }).map((match) => (
                    <div key={match.id} className="bg-white p-4 rounded-xl border border-slate-200 shadow-sm flex items-center space-x-3">
                      <img src={match.image} alt={match.name} className="w-12 h-12 rounded-lg object-cover" />
                      <div>
                        <h5 className="font-serif font-bold text-[#0B2545] text-sm">{match.name}</h5>
                        <p className="text-[10px] text-slate-500">{match.type}</p>
                      </div>
                    </div>
                  ))}
                </div>
              </div>
            </div>

            {/* CUSTOM ITINERARY BUILDER */}
            <div className="bg-[#0B2545] text-white rounded-3xl p-8 shadow-2xl border border-[#139A8C]/30 space-y-8">
              <div className="border-b border-slate-700/60 pb-4">
                <span className="text-xs font-bold text-[#139A8C] uppercase tracking-widest">Custom Trip Generator</span>
                <h2 className="text-3xl font-serif font-bold mt-1">Suggested Travel Itinerary Builder</h2>
                <p className="text-slate-300 text-sm mt-1">
                  Configure your duration to view a balanced daily breakdown.
                </p>
              </div>

              {/* CONTROLS */}
              <div className="grid grid-cols-1 sm:grid-cols-2 gap-6">
                <div>
                  <label className="block text-xs font-bold uppercase tracking-wider text-slate-300 mb-2">Trip Duration:</label>
                  <div className="grid grid-cols-3 gap-2">
                    {['2-3', '4-5', '6+'].map((days) => (
                      <button
                        key={days}
                        onClick={() => setItinDays(days)}
                        className={`py-2 rounded-xl text-xs font-bold border transition-colors ${
                          itinDays === days
                            ? 'bg-[#EE9B00] text-slate-900 border-[#EE9B00]'
                            : 'bg-white/10 text-white border-white/20 hover:bg-white/20'
                        }`}
                      >
                        {days} Days
                      </button>
                    ))}
                  </div>
                </div>

                <div>
                  <label className="block text-xs font-bold uppercase tracking-wider text-slate-300 mb-2">Travel Pace:</label>
                  <div className="grid grid-cols-2 gap-2">
                    {['balanced', 'packed'].map((p) => (
                      <button
                        key={p}
                        onClick={() => setItinPace(p)}
                        className={`py-2 rounded-xl text-xs font-bold border transition-colors capitalize ${
                          itinPace === p
                            ? 'bg-[#139A8C] text-white border-[#139A8C]'
                            : 'bg-white/10 text-white border-white/20 hover:bg-white/20'
                        }`}
                      >
                        {p} Pace
                      </button>
                    ))}
                  </div>
                </div>
              </div>

              {/* TIMELINE DISPLAY */}
              <div className="space-y-6 pt-4">
                {[
                  {
                    day: 'Day 1',
                    title: 'Arrival in Bongao & Sacred Mountain Trek',
                    morning: 'Land at Sanga-Sanga Airport (TWT). Hotel check-in at Bongao town.',
                    afternoon: 'Guided hike up Bud Bongao Peak. Offer bananas to macaques at the summit.',
                    evening: 'Sunset sea breeze and seafood dinner at Bihing Tahik.'
                  },
                  {
                    day: 'Day 2',
                    title: 'Simunul Island Heritage Trip',
                    morning: 'Board passenger launch at Chinese Pier to Simunul Island.',
                    afternoon: 'Visit Sheik Karimul Makhdum Mosque (Built 1380 AD). Relax at Tampakan Beach.',
                    evening: 'Return to Bongao. Dinner at local Satti eatery.'
                  },
                  ...(itinDays !== '2-3' ? [{
                    day: 'Day 3',
                    title: 'Sitangkai - The Venice of the South',
                    morning: 'Early speedboat to Sitangkai aquatic stilt town.',
                    afternoon: 'Tour canal waterways by wooden boat; visit seaweed farming communities.',
                    evening: 'Overnight homestay or return to Bongao town base.'
                  }] : []),
                  ...(itinDays === '6+' ? [{
                    day: 'Day 4',
                    title: 'Sibutu Craftsmanship & Boat Building',
                    morning: 'Travel to Sibutu to witness master boat builders crafting wooden kumpit vessels.',
                    afternoon: 'Explore natural freshwater springs and local coastal reefs.',
                    evening: 'Relaxing evening in Sibutu community.'
                  }] : [])
                ].map((item, idx) => (
                  <div key={idx} className="bg-white/10 backdrop-blur-md p-6 rounded-2xl border border-white/15 space-y-3">
                    <div className="flex items-center space-x-3">
                      <span className="bg-[#EE9B00] text-slate-900 text-xs font-extrabold px-3 py-1 rounded-full uppercase">
                        {item.day}
                      </span>
                      <h4 className="text-xl font-serif font-bold text-white">{item.title}</h4>
                    </div>
                    <div className="grid grid-cols-1 md:grid-cols-3 gap-4 pt-2 text-xs text-slate-200">
                      <div className="bg-black/20 p-3 rounded-xl border border-white/10">
                        <strong className="text-[#EE9B00] block mb-1">Morning:</strong>
                        <p>{item.morning}</p>
                      </div>
                      <div className="bg-black/20 p-3 rounded-xl border border-white/10">
                        <strong className="text-[#139A8C] block mb-1">Afternoon:</strong>
                        <p>{item.afternoon}</p>
                      </div>
                      <div className="bg-black/20 p-3 rounded-xl border border-white/10">
                        <strong className="text-slate-300 block mb-1">Evening:</strong>
                        <p>{item.evening}</p>
                      </div>
                    </div>
                  </div>
                ))}
              </div>

            </div>

          </div>
        )}

        {/* ================= 5. TRAVEL BUDGET CALCULATOR ================= */}
        {activeTab === 'budget' && (
          <div className="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8 py-10 space-y-8">
            <div className="text-center space-y-2">
              <span className="text-xs font-bold text-[#139A8C] uppercase tracking-widest">Financial Trip Planning</span>
              <h2 className="text-3xl font-serif font-bold text-[#0B2545]">Travel Budget Estimator</h2>
              <p className="text-slate-600 text-sm max-w-xl mx-auto">
                Estimate expected accommodation, meals, boat transfers, and emergency fees for your Tawi-Tawi journey.
              </p>
            </div>

            <div className="bg-white rounded-3xl p-8 border border-slate-200 shadow-xl space-y-8">
              
              {/* SLIDERS & CONTROLS */}
              <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
                
                <div className="space-y-2">
                  <div className="flex justify-between text-xs font-bold text-[#0B2545]">
                    <span>Number of Travelers:</span>
                    <span className="text-[#139A8C] text-sm">{calcTravelers} Person(s)</span>
                  </div>
                  <input 
                    type="range" 
                    min="1" 
                    max="10" 
                    value={calcTravelers} 
                    onChange={(e) => setCalcTravelers(Number(e.target.value))}
                    className="w-full accent-[#139A8C]" 
                  />
                </div>

                <div className="space-y-2">
                  <div className="flex justify-between text-xs font-bold text-[#0B2545]">
                    <span>Length of Stay:</span>
                    <span className="text-[#139A8C] text-sm">{calcDays} Days</span>
                  </div>
                  <input 
                    type="range" 
                    min="2" 
                    max="10" 
                    value={calcDays} 
                    onChange={(e) => setCalcDays(Number(e.target.value))}
                    className="w-full accent-[#139A8C]" 
                  />
                </div>

                <div className="space-y-2">
                  <label className="block text-xs font-bold text-[#0B2545]">Lodging Category:</label>
                  <select 
                    value={calcLodging} 
                    onChange={(e) => setCalcLodging(e.target.value)}
                    className="w-full bg-[#EEF4F8] border border-slate-300 rounded-xl p-3 text-xs font-semibold text-[#0B2545]"
                  >
                    <option value="budget">Budget Inn / Homestay (~₱1,000/night)</option>
                    <option value="mid">Mid-Range Hotel (~₱2,500/night)</option>
                    <option value="luxury">Resort / Premier Hotel (~₱4,500/night)</option>
                  </select>
                </div>

                <div className="space-y-2">
                  <label className="block text-xs font-bold text-[#0B2545]">Island Boat Transfers:</label>
                  <select 
                    value={calcBoats} 
                    onChange={(e) => setCalcBoats(e.target.value)}
                    className="w-full bg-[#EEF4F8] border border-slate-300 rounded-xl p-3 text-xs font-semibold text-[#0B2545]"
                  >
                    <option value="shared">Shared Public Launch (~₱800/day)</option>
                    <option value="private">Private Express Speedboat (~₱3,500/day)</option>
                  </select>
                </div>

              </div>

              {/* BREAKDOWN DISPLAY */}
              <div className="bg-[#0B2545] text-white rounded-2xl p-6 space-y-6">
                <h3 className="text-lg font-serif font-bold text-[#EE9B00] border-b border-slate-700 pb-3">
                  Estimated Cost Breakdown
                </h3>

                <div className="grid grid-cols-2 sm:grid-cols-4 gap-4 text-xs">
                  <div className="bg-white/10 p-3 rounded-xl border border-white/10">
                    <span className="text-slate-400 block">Accommodations</span>
                    <strong className="text-base text-white">₱{calculatedBudget.lodging.toLocaleString()}</strong>
                  </div>
                  <div className="bg-white/10 p-3 rounded-xl border border-white/10">
                    <span className="text-slate-400 block">Food & Dining</span>
                    <strong className="text-base text-white">₱{calculatedBudget.food.toLocaleString()}</strong>
                  </div>
                  <div className="bg-white/10 p-3 rounded-xl border border-white/10">
                    <span className="text-slate-400 block">Boat Transfers</span>
                    <strong className="text-base text-white">₱{calculatedBudget.boats.toLocaleString()}</strong>
                  </div>
                  <div className="bg-white/10 p-3 rounded-xl border border-white/10">
                    <span className="text-slate-400 block">Fees & Misc</span>
                    <strong className="text-base text-white">₱{calculatedBudget.misc.toLocaleString()}</strong>
                  </div>
                </div>

                <div className="pt-4 border-t border-slate-700 flex flex-col sm:flex-row justify-between items-center gap-4">
                  <div>
                    <span className="text-xs text-slate-300">Estimated Total Group Expense:</span>
                    <p className="text-3xl font-serif font-bold text-[#139A8C]">₱{calculatedBudget.grandTotal.toLocaleString()}</p>
                  </div>
                  <div className="text-right">
                    <span className="text-xs text-slate-300">Cost Per Person:</span>
                    <p className="text-xl font-bold text-[#EE9B00]">₱{calculatedBudget.perPerson.toLocaleString()}</p>
                  </div>
                </div>
              </div>

              <div className="text-[11px] text-slate-500 bg-[#F4F1EA] p-4 rounded-xl border border-slate-200">
                <strong>Disclaimer:</strong> Prices are based on verified local estimates and average seasonal rates. Bring sufficient cash in Philippine Pesos (PHP) as ATMs are located primarily in Bongao town center.
              </div>

            </div>
          </div>
        )}

        {/* ================= 6. WHERE TO STAY ================= */}
        {activeTab === 'stay' && (
          <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10 space-y-8">
            <div className="border-b border-slate-300 pb-4">
              <span className="text-xs font-bold text-[#139A8C] uppercase tracking-widest">Accommodations Directory</span>
              <h2 className="text-3xl font-serif font-bold text-[#0B2545] mt-1">Where to Stay in Tawi-Tawi</h2>
              <p className="text-slate-600 text-sm mt-1">
                Verified hotels, eco-resorts, inns, and community homestays across Bongao and outer islands.
              </p>
            </div>

            <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-2 gap-8">
              {ACCOMMODATIONS_DATA.map((acc) => (
                <div key={acc.id} className="bg-white rounded-3xl overflow-hidden border border-slate-200 shadow-lg flex flex-col md:flex-row">
                  <div className="md:w-1/2 relative h-60 md:h-auto">
                    <img src={acc.image} alt={acc.name} className="w-full h-full object-cover" />
                    <span className="absolute top-3 left-3 bg-[#0B2545] text-white text-[10px] font-bold px-2.5 py-1 rounded-full uppercase">
                      {acc.island} Island
                    </span>
                  </div>
                  <div className="md:w-1/2 p-6 flex flex-col justify-between space-y-4">
                    <div>
                      <div className="flex justify-between items-start">
                        <h3 className="text-xl font-serif font-bold text-[#0B2545]">{acc.name}</h3>
                        <span className="bg-[#EEF4F8] text-[#139A8C] text-xs font-bold px-2 py-0.5 rounded-md">
                          ★ {acc.rating}
                        </span>
                      </div>
                      <p className="text-xs font-semibold text-[#CA6702] mt-1">{acc.type} • {acc.price}</p>
                      <p className="text-xs text-slate-600 mt-2 leading-relaxed">{acc.desc}</p>
                    </div>

                    <div className="space-y-3 pt-2 border-t border-slate-100">
                      <div className="flex flex-wrap gap-1">
                        {acc.features.map((f, idx) => (
                          <span key={idx} className="text-[10px] bg-slate-100 text-slate-700 px-2 py-0.5 rounded">
                            {f}
                          </span>
                        ))}
                      </div>

                      <div className="text-xs text-slate-700 bg-[#EEF4F8] p-2.5 rounded-xl border border-[#139A8C]/20 flex items-center justify-between">
                        <span><strong>Contact:</strong> {acc.contact}</span>
                      </div>
                    </div>
                  </div>
                </div>
              ))}
            </div>
          </div>
        )}

        {/* ================= 7. PEOPLE & CULTURE ================= */}
        {activeTab === 'culture' && (
          <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10 space-y-12">
            
            <div className="border-b border-slate-300 pb-4">
              <span className="text-xs font-bold text-[#139A8C] uppercase tracking-widest">Cultural Mosaic</span>
              <h2 className="text-3xl font-serif font-bold text-[#0B2545] mt-1">The People & Heritage of Tawi-Tawi</h2>
              <p className="text-slate-600 text-sm mt-1">
                Discover the traditions, languages, attire, and craftsmanship of the Sama, Tausug, and Jama Mapun communities.
              </p>
            </div>

            {/* ETHNIC GROUPS CARDS */}
            <div className="grid grid-cols-1 lg:grid-cols-3 gap-8">
              {CULTURES_DATA.map((item) => (
                <div key={item.id} className="bg-white rounded-3xl overflow-hidden border border-slate-200 shadow-xl flex flex-col justify-between">
                  <div className="relative h-56">
                    <img src={item.image} alt={item.group} className="w-full h-full object-cover" />
                    <div className="absolute inset-0 bg-gradient-to-t from-[#0B2545] via-transparent to-transparent"></div>
                    <h3 className="absolute bottom-4 left-4 text-2xl font-serif font-bold text-white">{item.group}</h3>
                  </div>

                  <div className="p-6 space-y-4 text-xs text-slate-700 flex-grow">
                    <div>
                      <strong className="text-[#0B2545] block font-semibold">Language Spoken:</strong>
                      <p className="text-slate-600">{item.language}</p>
                    </div>
                    <div>
                      <strong className="text-[#0B2545] block font-semibold">Cultural Heritage:</strong>
                      <p className="text-slate-600 leading-relaxed">{item.heritage}</p>
                    </div>
                    <div>
                      <strong className="text-[#0B2545] block font-semibold">Traditional Crafts & Textiles:</strong>
                      <p className="text-slate-600">{item.crafts}</p>
                    </div>
                    <div>
                      <strong className="text-[#0B2545] block font-semibold">Culinary Legacy:</strong>
                      <p className="text-slate-600">{item.food}</p>
                    </div>
                  </div>
                </div>
              ))}
            </div>

            {/* CULTURAL ETIQUETTE FOR VISITORS */}
            <div className="bg-[#0B2545] text-white rounded-3xl p-8 border border-[#139A8C]/30 space-y-6">
              <div className="flex items-center space-x-3">
                <ShieldAlert className="w-8 h-8 text-[#EE9B00]" />
                <h3 className="text-2xl font-serif font-bold">Cultural Etiquette Guidelines for Visitors</h3>
              </div>

              <div className="grid grid-cols-1 md:grid-cols-2 gap-6 text-xs text-slate-200">
                <div className="bg-white/10 p-4 rounded-2xl border border-white/10 space-y-2">
                  <h4 className="font-bold text-[#EE9B00] text-sm">1. Dress Modestly at Sacred Sites</h4>
                  <p>When visiting Sheik Karimul Makhdum Mosque or community sites, cover shoulders and legs. Female visitors are requested to wear a headscarf inside mosques.</p>
                </div>
                <div className="bg-white/10 p-4 rounded-2xl border border-white/10 space-y-2">
                  <h4 className="font-bold text-[#139A8C] text-sm">2. Ask Permission before Photography</h4>
                  <p>Always ask warmly before photographing elders, children, or weavers at work. Respect their privacy and choices.</p>
                </div>
                <div className="bg-white/10 p-4 rounded-2xl border border-white/10 space-y-2">
                  <h4 className="font-bold text-[#EE9B00] text-sm">3. Support Local Artisans</h4>
                  <p>Purchase Tepoh mats, Pis Syabit, and local handicrafts directly from weavers and community craftsmen at fair asking prices.</p>
                </div>
                <div className="bg-white/10 p-4 rounded-2xl border border-white/10 space-y-2">
                  <h4 className="font-bold text-[#139A8C] text-sm">4. Environmental Mindfulness</h4>
                  <p>Do not step on coral reefs, disturb nesting sea turtles in Baguan Island, or leave trash on Bud Bongao.</p>
                </div>
              </div>
            </div>

          </div>
        )}

        {/* ================= 8. KAMARDIKAAN FESTIVAL ================= */}
        {activeTab === 'kamardikaan' && (
          <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10 space-y-12">
            
            {/* HERO BANNER FOR FESTIVAL */}
            <div className="relative rounded-3xl overflow-hidden bg-gradient-to-r from-[#0B2545] via-[#139A8C] to-[#0B2545] p-8 lg:p-12 text-white shadow-2xl">
              <div className="max-w-3xl space-y-4 relative z-10">
                <span className="bg-[#EE9B00] text-slate-900 font-extrabold px-3 py-1 rounded-full text-xs uppercase tracking-widest">
                  Annual Provincial Festival
                </span>
                <h2 className="text-3xl sm:text-5xl font-serif font-extrabold leading-tight">
                  KAMARDIKAAN SIN TAWI-TAWI
                </h2>
                <p className="text-slate-100 text-sm sm:text-base leading-relaxed">
                  Celebrating freedom, cultural unity, maritime heritage, and the official founding anniversary of Tawi-Tawi province every September.
                </p>
              </div>
            </div>

            {/* HIGHLIGHTS & ACTIVITIES */}
            <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
              {[
                {
                  title: 'Agal-Agal (Seaweed) Parade',
                  desc: 'Colorful float parades honoring Tawi-Tawi\'s title as the "Seaweed Capital of the Philippines."',
                  image: 'https://images.unsplash.com/photo-1544551763-46a013bb70d5?auto=format&fit=crop&w=600&q=80'
                },
                {
                  title: 'Traditional Dance Performances',
                  desc: 'Masterful performances of Pangalay dances featuring rhythmic brass fan movements.',
                  image: 'https://images.unsplash.com/photo-1606744824163-985d376605aa?auto=format&fit=crop&w=600&q=80'
                },
                {
                  title: 'Lepa Regatta Flotilla',
                  desc: 'A grand sea showcase of traditionally painted and adorned outrigger Lepa houseboats.',
                  image: 'https://images.unsplash.com/photo-1507525428034-b723cf961d3e?auto=format&fit=crop&w=600&q=80'
                }
              ].map((feat, idx) => (
                <div key={idx} className="bg-white rounded-2xl overflow-hidden border border-slate-200 shadow-md">
                  <img src={feat.image} alt={feat.title} className="w-full h-44 object-cover" />
                  <div className="p-5 space-y-2">
                    <h3 className="text-lg font-serif font-bold text-[#0B2545]">{feat.title}</h3>
                    <p className="text-xs text-slate-600 leading-relaxed">{feat.desc}</p>
                  </div>
                </div>
              ))}
            </div>

          </div>
        )}

        {/* ================= 9. FOOD & CRAFTS ================= */}
        {activeTab === 'food' && (
          <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10 space-y-12">
            
            <div className="border-b border-slate-300 pb-4">
              <span className="text-xs font-bold text-[#139A8C] uppercase tracking-widest">Taste of Tawi-Tawi & Local Souvenirs</span>
              <h2 className="text-3xl font-serif font-bold text-[#0B2545] mt-1">Culinary Heritage & Artisan Crafts</h2>
              <p className="text-slate-600 text-sm mt-1">
                Explore signature Tausug and Sama delicacies, fresh seafood, and hand-woven keepsakes.
              </p>
            </div>

            {/* FOOD GALLERY */}
            <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
              {FOOD_DATA.map((dish, idx) => (
                <div key={idx} className="bg-white rounded-2xl overflow-hidden border border-slate-200 shadow-md flex flex-col justify-between">
                  <div className="relative h-44">
                    <img src={dish.image} alt={dish.name} className="w-full h-full object-cover" />
                    <span className="absolute top-2 left-2 bg-[#CA6702] text-white text-[10px] font-bold px-2 py-0.5 rounded-full uppercase">
                      {dish.type}
                    </span>
                  </div>
                  <div className="p-5 space-y-3 flex-grow flex flex-col justify-between">
                    <div>
                      <h3 className="text-lg font-serif font-bold text-[#0B2545]">{dish.name}</h3>
                      <p className="text-xs text-slate-600 mt-1 leading-relaxed">{dish.desc}</p>
                    </div>
                    <div className="pt-2 border-t border-slate-100 text-[11px] text-slate-500">
                      <strong>Key Ingredients:</strong> {dish.ingredients}
                    </div>
                  </div>
                </div>
              ))}
            </div>

            {/* SOUVENIR GUIDE */}
            <div className="bg-[#EEF4F8] rounded-3xl p-8 border border-[#139A8C]/30 space-y-6">
              <h3 className="text-2xl font-serif font-bold text-[#0B2545]">Authentic Souvenirs to Take Home</h3>
              <div className="grid grid-cols-1 md:grid-cols-3 gap-6 text-xs text-slate-700">
                <div className="bg-white p-5 rounded-2xl border border-slate-200 space-y-2">
                  <h4 className="font-bold text-[#0B2545] text-sm">1. Handwoven Tepoh Mats</h4>
                  <p>Intricately dyed pandan mats woven by master Sama weavers in Tandubas and Laminusa.</p>
                </div>
                <div className="bg-white p-5 rounded-2xl border border-slate-200 space-y-2">
                  <h4 className="font-bold text-[#0B2545] text-sm">2. Wooden Miniature Lepa Boats</h4>
                  <p>Hand-carved replica houseboats made by traditional Sibutu craftsmen.</p>
                </div>
                <div className="bg-white p-5 rounded-2xl border border-slate-200 space-y-2">
                  <h4 className="font-bold text-[#0B2545] text-sm">3. Local Seaweed Products</h4>
                  <p>Processed dried seaweed snacks and gel products from the seaweed farms of Sitangkai.</p>
                </div>
              </div>
            </div>

          </div>
        )}

        {/* ================= 10. BEFORE YOU VISIT / TRAVEL GUIDE ================= */}
        {activeTab === 'guide' && (
          <div className="max-w-5xl mx-auto px-4 sm:px-6 lg:px-8 py-10 space-y-10">
            
            <div className="border-b border-slate-300 pb-4 text-center">
              <span className="text-xs font-bold text-[#139A8C] uppercase tracking-widest">Essential Travel Guide</span>
              <h2 className="text-3xl font-serif font-bold text-[#0B2545] mt-1">Before You Visit Tawi-Tawi</h2>
              <p className="text-slate-600 text-sm max-w-xl mx-auto mt-1">
                Important transportation logistics, best weather windows, money, and safety recommendations.
              </p>
            </div>

            <div className="space-y-6">
              {[
                {
                  title: 'How to Get to Tawi-Tawi',
                  icon: Navigation,
                  content: 'Fly directly into Sanga-Sanga Airport (TWT) near Bongao via domestic flights operating daily from Zamboanga City (ZAM). Passenger ferries also connect Zamboanga Port to Bongao.'
                },
                {
                  title: 'Best Time to Visit',
                  icon: Sun,
                  content: 'The ideal dry season runs from March through October, offering calm seas for island hopping. Check marine forecasts before booking outer-island boats.'
                },
                {
                  title: 'ATMs & Cash Requirements',
                  icon: DollarSign,
                  content: 'ATMs (Landbank, PNB) are available in Bongao town center. Always carry sufficient Philippine Peso (PHP) cash when traveling to outer islands like Simunul or Sitangkai.'
                },
                {
                  title: 'Mobile Signal & Connectivity',
                  icon: Sparkles,
                  content: '4G mobile signal is stable in main Bongao town and Sanga-Sanga. Connectivity can be limited in remote island groups.'
                }
              ].map((item, idx) => (
                <div key={idx} className="bg-white rounded-2xl p-6 border border-slate-200 shadow-sm flex items-start space-x-4">
                  <div className="p-3 bg-[#139A8C] text-white rounded-xl shrink-0">
                    <item.icon className="w-6 h-6" />
                  </div>
                  <div>
                    <h3 className="text-lg font-serif font-bold text-[#0B2545]">{item.title}</h3>
                    <p className="text-xs text-slate-600 mt-1 leading-relaxed">{item.content}</p>
                  </div>
                </div>
              ))}
            </div>

          </div>
        )}

      </main>

      {/* --- FOOTER --- */}
      <footer className="bg-[#0B2545] text-white border-t border-[#139A8C]/30 py-12 mt-16">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 space-y-8">
          <div className="flex flex-col md:flex-row justify-between items-center gap-6">
            <div className="flex items-center space-x-3">
              <div className="w-10 h-10 rounded-full bg-[#139A8C] flex items-center justify-center text-white font-bold">
                <Anchor className="w-5 h-5" />
              </div>
              <div>
                <h3 className="font-serif font-bold text-lg">TAWI-TAWI ISLANDS</h3>
                <p className="text-[10px] text-slate-400 uppercase tracking-widest">Official Visitor Information Guide</p>
              </div>
            </div>

            <div className="flex flex-wrap justify-center gap-6 text-xs text-slate-300">
              <button onClick={() => setActiveTab('islands')} className="hover:text-[#EE9B00]">Explore Islands</button>
              <button onClick={() => setActiveTab('map')} className="hover:text-[#EE9B00]">Interactive Map</button>
              <button onClick={() => setActiveTab('planner')} className="hover:text-[#EE9B00]">Trip Planner</button>
              <button onClick={() => setActiveTab('culture')} className="hover:text-[#EE9B00]">Culture</button>
              <button onClick={() => setActiveTab('guide')} className="hover:text-[#EE9B00]">Travel Logistics</button>
            </div>
          </div>

          <div className="border-t border-slate-800 pt-6 text-center text-xs text-slate-400">
            <p>© {new Date().getFullYear()} Tawi-Tawi Islands Interactive Travel Directory. Prepared for visitors, students, and eco-travelers.</p>
          </div>
        </div>
      </footer>

    </div>
  );
}

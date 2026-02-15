import React, { useState, useEffect, useMemo } from 'react';
import { 
  ShoppingBag, 
  Menu, 
  X, 
  ChevronRight, 
  Star, 
  ArrowRight, 
  Instagram, 
  Facebook, 
  Twitter,
  Search,
  Heart,
  Plus,
  Minus,
  Trash2,
  CheckCircle2
} from 'lucide-react';

/**
 * MALAK STORE - Premium E-commerce
 * Optimized Imagery: Faceless Women's Fashion & Architectural Hero
 */

const App = () => {
  const [isMenuOpen, setIsMenuOpen] = useState(false);
  const [isCartOpen, setIsCartOpen] = useState(false);
  const [isScrolled, setIsScrolled] = useState(false);
  const [activeCategory, setActiveCategory] = useState('All');
  const [searchQuery, setSearchQuery] = useState('');
  const [isSearching, setIsSearching] = useState(false);
  
  const [cart, setCart] = useState([]);
  const [wishlist, setWishlist] = useState([]);
  
  const [newsletterEmail, setNewsletterEmail] = useState('');
  const [subscribed, setSubscribed] = useState(false);

  const categories = ['All', 'Women', 'Men', 'Jewelry', 'Fragrance', 'Home'];

  const products = [
    // Women's Fashion (Faceless/Detail Focused)
    { id: 1, name: 'Silk Serenity Wrap', price: 240, category: 'Women', image: 'https://images.unsplash.com/photo-1581044777550-4cfa60707c03?auto=format&fit=crop&q=80&w=1000' },
    { id: 7, name: 'Athena Pleated Gown', price: 420, category: 'Women', image: 'https://images.unsplash.com/photo-1490481651871-ab68de25d43d?auto=format&fit=crop&q=80&w=1000' },
    { id: 8, name: 'Linen Resort Trousers', price: 180, category: 'Women', image: 'https://images.unsplash.com/photo-1509631179647-0177331693ae?auto=format&fit=crop&q=80&w=1000' },
    
    // Men's Fashion
    { id: 9, name: 'Tailored Wool Blazer', price: 550, category: 'Men', image: 'https://images.unsplash.com/photo-1507679799987-c73779587ccf?auto=format&fit=crop&q=80&w=1000' },
    { id: 10, name: 'Oxford Signature Shirt', price: 145, category: 'Men', image: 'https://images.unsplash.com/photo-1617137968427-85924c800a22?auto=format&fit=crop&q=80&w=1000' },
    { id: 11, name: 'Classic Chino Pant', price: 165, category: 'Men', image: 'https://images.unsplash.com/photo-1618886616039-60e458ad98c5?auto=format&fit=crop&q=80&w=1000' },
    
    // Jewelry
    { id: 2, name: 'Gold Petal Earrings', price: 185, category: 'Jewelry', image: 'https://images.unsplash.com/photo-1635767798638-3e25273a8236?auto=format&fit=crop&q=80&w=1000' },
    { id: 5, name: 'Minimalist Timepiece', price: 310, category: 'Jewelry', image: 'https://images.unsplash.com/photo-1522312346375-d1a52e2b99b3?auto=format&fit=crop&q=80&w=1000' },
    
    // Fragrance & Home
    { id: 3, name: 'Midnight Oud', price: 120, category: 'Fragrance', image: 'https://images.unsplash.com/photo-1541643600914-78b084683601?auto=format&fit=crop&q=80&w=1000' },
    { id: 4, name: 'Velvet Luna Chair', price: 890, category: 'Home', image: 'https://images.unsplash.com/photo-1592078615290-033ee584e267?auto=format&fit=crop&q=80&w=1000' },
  ];

  useEffect(() => {
    const handleScroll = () => setIsScrolled(window.scrollY > 50);
    window.addEventListener('scroll', handleScroll);
    return () => window.removeEventListener('scroll', handleScroll);
  }, []);

  const filteredProducts = useMemo(() => {
    return products.filter(p => {
      const matchesCategory = activeCategory === 'All' || p.category === activeCategory;
      const matchesSearch = p.name.toLowerCase().includes(searchQuery.toLowerCase());
      return matchesCategory && matchesSearch;
    });
  }, [activeCategory, searchQuery]);

  const addToCart = (product) => {
    setCart(prev => {
      const existing = prev.find(item => item.id === product.id);
      if (existing) {
        return prev.map(item => item.id === product.id ? { ...item, quantity: item.quantity + 1 } : item);
      }
      return [...prev, { ...product, quantity: 1 }];
    });
    setIsCartOpen(true);
  };

  const updateQuantity = (id, delta) => {
    setCart(prev => prev.map(item => {
      if (item.id === id) return { ...item, quantity: Math.max(1, item.quantity + delta) };
      return item;
    }));
  };

  const removeFromCart = (id) => setCart(prev => prev.filter(item => item.id !== id));
  const toggleWishlist = (id) => setWishlist(prev => prev.includes(id) ? prev.filter(i => i !== id) : [...prev, id]);
  const cartTotal = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
  const cartCount = cart.reduce((sum, item) => sum + item.quantity, 0);

  const handleNewsletterSubmit = (e) => {
    e.preventDefault();
    if (newsletterEmail) { setSubscribed(true); setTimeout(() => setSubscribed(false), 5000); setNewsletterEmail(''); }
  };

  return (
    <div className="min-h-screen bg-[#FDFCFB] text-[#2D2926] font-sans selection:bg-[#D4AF37] selection:text-white">
      
      {/* Navigation */}
      <nav className={`fixed w-full z-50 transition-all duration-500 px-6 py-4 ${
        isScrolled || isSearching ? 'bg-white/95 backdrop-blur-md shadow-sm' : 'bg-transparent'
      }`}>
        <div className="max-w-7xl mx-auto flex justify-between items-center">
          <div className="flex items-center gap-8">
            <button onClick={() => setIsMenuOpen(true)} className="lg:hidden p-2"><Menu size={24} /></button>
            <div className="hidden lg:flex gap-8 text-sm uppercase tracking-widest font-bold">
              <a href="#shop" className="hover:text-[#D4AF37] transition-colors">Shop</a>
              <a href="#about" className="hover:text-[#D4AF37] transition-colors">House of Malak</a>
            </div>
          </div>

          <h1 className="text-2xl md:text-3xl font-serif tracking-[0.3em] uppercase font-light cursor-pointer">
            Malak<span className="text-[#D4AF37]">.</span>
          </h1>

          <div className="flex items-center gap-4">
            <div className={`flex items-center bg-black/5 rounded-full px-4 py-2 transition-all ${isSearching ? 'w-48 md:w-64' : 'w-10 overflow-hidden md:w-48'}`}>
               <Search size={18} className="text-gray-500 shrink-0 cursor-pointer" onClick={() => setIsSearching(!isSearching)} />
               <input 
                type="text" placeholder="Search..." className="bg-transparent border-none outline-none text-xs ml-2 w-full font-medium"
                value={searchQuery} onChange={(e) => setSearchQuery(e.target.value)} onFocus={() => setIsSearching(true)}
               />
            </div>
            <div className="relative cursor-pointer p-2" onClick={() => setIsCartOpen(true)}>
              <ShoppingBag size={22} />
              {cartCount > 0 && <span className="absolute top-1 right-1 bg-[#D4AF37] text-white text-[10px] w-4 h-4 rounded-full flex items-center justify-center font-bold">{cartCount}</span>}
            </div>
          </div>
        </div>
      </nav>

      {/* Cart Sidebar */}
      <div className={`fixed inset-0 z-[100] transition-opacity duration-500 ${isCartOpen ? 'opacity-100 pointer-events-auto' : 'opacity-0 pointer-events-none'}`}>
        <div className="absolute inset-0 bg-black/40 backdrop-blur-sm" onClick={() => setIsCartOpen(false)} />
        <div className={`absolute right-0 top-0 h-full w-full max-w-md bg-white shadow-2xl transition-transform duration-500 p-8 flex flex-col ${isCartOpen ? 'translate-x-0' : 'translate-x-full'}`}>
          <div className="flex justify-between items-center mb-10">
            <h3 className="text-2xl font-serif">Bag ({cartCount})</h3>
            <button onClick={() => setIsCartOpen(false)} className="p-2"><X size={24} /></button>
          </div>
          <div className="flex-grow overflow-y-auto pr-2 no-scrollbar">
            {cart.map(item => (
              <div key={item.id} className="flex gap-4 mb-6 group">
                <div className="w-24 h-32 bg-gray-100 rounded-xl overflow-hidden shrink-0">
                  <img src={item.image} className="w-full h-full object-cover" alt={item.name} />
                </div>
                <div className="flex-grow flex flex-col justify-between py-1">
                  <div className="flex justify-between font-serif text-lg">
                    <h4>{item.name}</h4>
                    <button onClick={() => removeFromCart(item.id)}><Trash2 size={16} className="text-gray-300 hover:text-red-500" /></button>
                  </div>
                  <div className="flex justify-between items-center">
                    <div className="flex items-center border rounded-full px-2 py-1">
                      <button onClick={() => updateQuantity(item.id, -1)}><Minus size={12} /></button>
                      <span className="px-3 text-sm">{item.quantity}</span>
                      <button onClick={() => updateQuantity(item.id, 1)}><Plus size={12} /></button>
                    </div>
                    <p className="font-serif font-bold">${item.price * item.quantity}</p>
                  </div>
                </div>
              </div>
            ))}
          </div>
          {cart.length > 0 && (
            <div className="pt-8 border-t border-gray-100">
              <div className="flex justify-between items-end mb-6">
                <span className="text-xs tracking-widest text-gray-400 uppercase">Subtotal</span>
                <span className="text-2xl font-serif">${cartTotal}</span>
              </div>
              <button className="w-full bg-[#2D2926] text-white py-5 rounded-full uppercase text-xs tracking-[0.3em] font-bold hover:bg-[#D4AF37] transition-all">Complete Purchase</button>
            </div>
          )}
        </div>
      </div>

      {/* Hero Section - Architectural Masterpiece */}
      <section className="relative h-screen flex items-center justify-center overflow-hidden">
        <div className="absolute inset-0 z-0">
          <img 
            src="https://images.unsplash.com/photo-1441986300917-64674bd600d8?auto=format&fit=crop&q=80&w=2000" 
            className="w-full h-full object-cover animate-subtle-zoom brightness-90"
            alt="Premium Interior Architecture"
          />
          <div className="absolute inset-0 bg-gradient-to-b from-black/20 via-transparent to-[#FDFCFB]"></div>
        </div>
        <div className="relative z-10 text-center px-4 max-w-5xl animate-in fade-in duration-1000">
          <p className="text-[#D4AF37] uppercase tracking-[0.6em] mb-6 text-xs font-black">Refined Living · Est. 2024</p>
          <h2 className="text-6xl md:text-[10rem] font-serif font-light mb-8 leading-none tracking-tighter">
            Pure <span className="italic text-[#D4AF37]">Grace</span>
          </h2>
          <a href="#shop" className="inline-block">
            <button className="px-14 py-6 bg-[#2D2926] text-white rounded-full uppercase text-xs tracking-[0.4em] font-bold hover:scale-105 transition-transform shadow-2xl">
              Shop The Curation
            </button>
          </a>
        </div>
      </section>

      {/* Shop Section */}
      <section id="shop" className="py-32 px-6 max-w-7xl mx-auto">
        <div className="flex flex-col md:flex-row justify-between items-end mb-20 gap-8">
          <div>
            <h3 className="text-4xl font-serif mb-4">Select Collections</h3>
            <p className="text-gray-400 text-lg font-light">Elegance defined by silhouette and superior craftsmanship.</p>
          </div>
          <div className="flex gap-3 overflow-x-auto pb-4 no-scrollbar w-full md:w-auto">
            {categories.map(cat => (
              <button 
                key={cat} onClick={() => setActiveCategory(cat)}
                className={`px-8 py-3 rounded-full text-[10px] uppercase tracking-widest transition-all font-black border ${
                  activeCategory === cat ? 'bg-[#2D2926] text-white' : 'bg-white border-gray-100 text-gray-400 hover:border-[#D4AF37] hover:text-[#D4AF37]'
                }`}
              >
                {cat}
              </button>
            ))}
          </div>
        </div>

        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-x-12 gap-y-24">
          {filteredProducts.map((product) => (
            <div key={product.id} className="group cursor-pointer">
              <div className="relative aspect-[3/4] overflow-hidden rounded-3xl mb-6 shadow-xl bg-gray-100">
                <img src={product.image} className="w-full h-full object-cover transition-transform duration-1000 group-hover:scale-110" alt={product.name} />
                <div className="absolute inset-0 bg-black/5 opacity-0 group-hover:opacity-100 transition-opacity" />
                <div className="absolute bottom-8 left-0 right-0 flex justify-center px-8 translate-y-24 group-hover:translate-y-0 transition-all duration-500">
                  <button onClick={(e) => { e.stopPropagation(); addToCart(product); }} className="w-full bg-white/95 backdrop-blur py-4 rounded-full text-[10px] uppercase tracking-widest font-black shadow-xl hover:bg-[#D4AF37] hover:text-white transition-all">Add To Bag</button>
                </div>
                <button onClick={(e) => { e.stopPropagation(); toggleWishlist(product.id); }} className="absolute top-6 right-6 p-3 bg-white/90 backdrop-blur rounded-full shadow-md">
                  <Heart size={18} className={`${wishlist.includes(product.id) ? 'fill-red-500 text-red-500' : 'text-gray-400'}`} />
                </button>
              </div>
              <div className="flex justify-between items-start px-2">
                <div>
                  <p className="text-[9px] uppercase tracking-[0.3em] text-[#D4AF37] mb-2 font-black">{product.category}</p>
                  <h4 className="text-xl font-serif">{product.name}</h4>
                </div>
                <p className="font-serif text-xl">${product.price}</p>
              </div>
            </div>
          ))}
        </div>
      </section>

      {/* Footer & Newsletter */}
      <footer className="bg-[#2D2926] text-white py-32 px-6">
        <div className="max-w-7xl mx-auto">
          <div className="grid grid-cols-1 lg:grid-cols-2 gap-20 mb-24 items-center">
            <div>
              <h3 className="text-5xl font-serif mb-6 leading-tight">Join the <br />Private Circle</h3>
              <p className="text-gray-400 text-lg font-light mb-10">Access exclusive releases and private invitations.</p>
              <form onSubmit={handleNewsletterSubmit} className="relative flex border-b border-gray-700 py-6 max-w-md focus-within:border-[#D4AF37] transition-all">
                <input 
                  type="email" placeholder="EMAIL ADDRESS" className="bg-transparent flex-grow outline-none text-xs tracking-widest placeholder:text-gray-700"
                  value={newsletterEmail} onChange={(e) => setNewsletterEmail(e.target.value)}
                />
                <button type="submit" className="uppercase text-[10px] font-black tracking-[0.4em] hover:text-[#D4AF37]">Enter</button>
              </form>
              {subscribed && <p className="text-[#D4AF37] text-xs mt-4 uppercase tracking-widest font-bold">Successfully Registered.</p>}
            </div>
            <div className="grid grid-cols-2 gap-12 text-sm uppercase tracking-widest text-gray-500 font-bold">
              <div className="space-y-6">
                <p className="text-white text-[11px] tracking-[0.4em] mb-10">House</p>
                <p className="hover:text-white cursor-pointer">Archive</p>
                <p className="hover:text-white cursor-pointer">Bespoke</p>
                <p className="hover:text-white cursor-pointer">Sustainability</p>
              </div>
              <div className="space-y-6">
                <p className="text-white text-[11px] tracking-[0.4em] mb-10">Connect</p>
                <p className="hover:text-white cursor-pointer">Instagram</p>
                <p className="hover:text-white cursor-pointer">Concierge</p>
                <p className="hover:text-white cursor-pointer">Boutiques</p>
              </div>
            </div>
          </div>
          <div className="pt-20 border-t border-gray-800 flex flex-col md:flex-row justify-between items-center gap-6 text-[10px] uppercase tracking-[0.4em] text-gray-600 font-black">
            <p>© 2024 Malak Maison. All Rights Reserved.</p>
            <div className="flex gap-10"><a href="#">Privacy</a><a href="#">Terms</a><a href="#">Cookies</a></div>
          </div>
        </div>
      </footer>

      <style>{`
        @import url('https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,700;1,400&family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap');
        body { font-family: 'Plus Jakarta Sans', sans-serif; scroll-behavior: smooth; }
        .font-serif { font-family: 'Playfair Display', serif; }
        .no-scrollbar::-webkit-scrollbar { display: none; }
        @keyframes subtle-zoom { from { transform: scale(1); } to { transform: scale(1.08); } }
        .animate-subtle-zoom { animation: subtle-zoom 20s ease-in-out infinite alternate; }
        @keyframes fade-in { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .animate-in { animation: fade-in 1.2s ease-out forwards; }
      `}</style>
    </div>
  );
};

export default App;

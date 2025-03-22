# Balance Cleanse E-commerce Next.js Project

I'll create a modern Next.js e-commerce website based on balancecleanse.com with TypeScript, Tailwind CSS, and Next-UI with interactive hover effects.

```typescript
// File: package.json
{
  "name": "balance-cleanse-ecommerce",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  },
  "dependencies": {
    "@nextui-org/react": "^2.2.9",
    "next": "14.1.0",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "framer-motion": "^10.18.0",
    "react-icons": "^5.0.1",
    "typescript": "^5.3.3"
  },
  "devDependencies": {
    "@types/node": "^20.11.0",
    "@types/react": "^18.2.47",
    "@types/react-dom": "^18.2.18",
    "autoprefixer": "^10.4.16",
    "eslint": "^8.56.0",
    "eslint-config-next": "14.1.0",
    "postcss": "^8.4.33",
    "tailwindcss": "^3.4.1"
  }
}
```

```typescript
// File: tailwind.config.js
const { nextui } = require("@nextui-org/react");

module.exports = {
  content: [
    "./app/**/*.{js,ts,jsx,tsx}",
    "./pages/**/*.{js,ts,jsx,tsx}",
    "./components/**/*.{js,ts,jsx,tsx}",
    "./node_modules/@nextui-org/theme/dist/**/*.{js,ts,jsx,tsx}"
  ],
  theme: {
    extend: {
      colors: {
        primary: {
          DEFAULT: '#4a8f66', // Assuming a green color for Balance Cleanse
          dark: '#3a7252',
          light: '#6baf87',
        },
        secondary: {
          DEFAULT: '#f5f5f1', // Light natural color
        }
      },
      fontFamily: {
        sans: ['Inter', 'sans-serif'],
        display: ['Montserrat', 'sans-serif'],
      },
      animation: {
        'fade-in': 'fadeIn 0.5s ease-in-out',
        'slide-up': 'slideUp 0.6s ease-out',
      },
      keyframes: {
        fadeIn: {
          '0%': { opacity: '0' },
          '100%': { opacity: '1' },
        },
        slideUp: {
          '0%': { transform: 'translateY(20px)', opacity: '0' },
          '100%': { transform: 'translateY(0)', opacity: '1' },
        },
      },
    },
  },
  darkMode: "class",
  plugins: [nextui()],
}
```

```typescript
// File: app/layout.tsx
import { Providers } from "./providers";
import { Inter, Montserrat } from 'next/font/google';
import { Navbar } from "@/components/Navbar";
import { Footer } from "@/components/Footer";
import "./globals.css";

const inter = Inter({ subsets: ['latin'], variable: '--font-inter' });
const montserrat = Montserrat({ subsets: ['latin'], variable: '--font-montserrat' });

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en" className={`${inter.variable} ${montserrat.variable}`}>
      <body>
        <Providers>
          <Navbar />
          <main className="min-h-screen pt-16">
            {children}
          </main>
          <Footer />
        </Providers>
      </body>
    </html>
  );
}
```

```typescript
// File: app/globals.css
@tailwind base;
@tailwind components;
@tailwind utilities;

:root {
  --foreground-rgb: 0, 0, 0;
  --background-start-rgb: 245, 245, 241;
  --background-end-rgb: 255, 255, 255;
}

body {
  color: rgb(var(--foreground-rgb));
  background: linear-gradient(
      to bottom,
      transparent,
      rgb(var(--background-end-rgb))
    )
    rgb(var(--background-start-rgb));
}

.hover-scale {
  transition: transform 0.3s ease;
}

.hover-scale:hover {
  transform: scale(1.03);
}

.hover-lift {
  transition: all 0.3s ease;
}

.hover-lift:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
}

.nav-link {
  position: relative;
}

.nav-link::after {
  content: '';
  position: absolute;
  width: 0;
  height: 2px;
  bottom: -2px;
  left: 0;
  background-color: #4a8f66;
  transition: width 0.3s ease;
}

.nav-link:hover::after {
  width: 100%;
}
```

```typescript
// File: app/providers.tsx
"use client";

import { NextUIProvider } from "@nextui-org/react";
import { ThemeProvider as NextThemesProvider } from "next-themes";

export function Providers({ children }: { children: React.ReactNode }) {
  return (
    <NextUIProvider>
      <NextThemesProvider attribute="class" defaultTheme="light">
        {children}
      </NextThemesProvider>
    </NextUIProvider>
  );
}
```

```typescript
// File: app/page.tsx
import { Hero } from "@/components/Hero";
import { FeaturedProducts } from "@/components/FeaturedProducts";
import { BenefitsSection } from "@/components/BenefitsSection";
import { TestimonialsSection } from "@/components/TestimonialsSection";
import { NewsletterSignup } from "@/components/NewsletterSignup";

export default function Home() {
  return (
    <div className="flex flex-col gap-12 md:gap-24">
      <Hero />
      <FeaturedProducts />
      <BenefitsSection />
      <TestimonialsSection />
      <NewsletterSignup />
    </div>
  );
}
```

```typescript
// File: components/Navbar.tsx
"use client";

import { useState } from "react";
import Link from "next/link";
import Image from "next/image";
import { Navbar as NextUINavbar, NavbarBrand, NavbarContent, NavbarItem, NavbarMenuToggle, NavbarMenu, NavbarMenuItem, Button, Badge } from "@nextui-org/react";
import { FaShoppingCart, FaUser, FaSearch } from "react-icons/fa";

export const Navbar = () => {
  const [isMenuOpen, setIsMenuOpen] = useState(false);
  const [cartCount, setCartCount] = useState(0);

  return (
    <NextUINavbar 
      isMenuOpen={isMenuOpen} 
      onMenuOpenChange={setIsMenuOpen}
      className="fixed top-0 z-50 bg-white/80 backdrop-blur-md border-b border-gray-100"
      maxWidth="xl"
    >
      <NavbarContent>
        <NavbarMenuToggle
          aria-label={isMenuOpen ? "Close menu" : "Open menu"}
          className="sm:hidden"
        />
        <NavbarBrand>
          <Link href="/" className="flex items-center gap-2">
            <Image 
              src="/logo.svg" 
              alt="Balance Cleanse Logo" 
              width={40} 
              height={40} 
              className="hover-scale"
            />
            <span className="font-display font-bold text-primary text-xl">
              Balance Cleanse
            </span>
          </Link>
        </NavbarBrand>
      </NavbarContent>

      <NavbarContent className="hidden sm:flex gap-6" justify="center">
        <NavbarItem>
          <Link href="/products" className="nav-link text-foreground">
            Products
          </Link>
        </NavbarItem>
        <NavbarItem>
          <Link href="/about" className="nav-link text-foreground">
            Our Story
          </Link>
        </NavbarItem>
        <NavbarItem>
          <Link href="/ingredients" className="nav-link text-foreground">
            Ingredients
          </Link>
        </NavbarItem>
        <NavbarItem>
          <Link href="/testimonials" className="nav-link text-foreground">
            Testimonials
          </Link>
        </NavbarItem>
        <NavbarItem>
          <Link href="/blog" className="nav-link text-foreground">
            Blog
          </Link>
        </NavbarItem>
      </NavbarContent>

      <NavbarContent justify="end">
        <NavbarItem>
          <Button isIconOnly variant="light" aria-label="Search">
            <FaSearch className="text-gray-700 hover:text-primary transition-colors" />
          </Button>
        </NavbarItem>
        <NavbarItem>
          <Button isIconOnly variant="light" aria-label="User account">
            <FaUser className="text-gray-700 hover:text-primary transition-colors" />
          </Button>
        </NavbarItem>
        <NavbarItem>
          <Badge content={cartCount} color="primary" size="sm" showOutline={false}>
            <Button 
              isIconOnly 
              variant="light" 
              aria-label="Cart" 
              className="hover-scale"
              as={Link} 
              href="/cart"
            >
              <FaShoppingCart className="text-gray-700 hover:text-primary transition-colors" />
            </Button>
          </Badge>
        </NavbarItem>
      </NavbarContent>

      <NavbarMenu className="pt-6">
        <NavbarMenuItem>
          <Link href="/products" className="w-full py-2 text-lg">
            Products
          </Link>
        </NavbarMenuItem>
        <NavbarMenuItem>
          <Link href="/about" className="w-full py-2 text-lg">
            Our Story
          </Link>
        </NavbarMenuItem>
        <NavbarMenuItem>
          <Link href="/ingredients" className="w-full py-2 text-lg">
            Ingredients
          </Link>
        </NavbarMenuItem>
        <NavbarMenuItem>
          <Link href="/testimonials" className="w-full py-2 text-lg">
            Testimonials
          </Link>
        </NavbarMenuItem>
        <NavbarMenuItem>
          <Link href="/blog" className="w-full py-2 text-lg">
            Blog
          </Link>
        </NavbarMenuItem>
      </NavbarMenu>
    </NextUINavbar>
  );
};
```

```typescript
// File: components/Hero.tsx
"use client";

import { Button } from "@nextui-org/react";
import { motion } from "framer-motion";
import Image from "next/image";
import Link from "next/link";

export const Hero = () => {
  return (
    <section className="relative w-full h-[90vh] flex items-center">
      {/* Background Image */}
      <div className="absolute inset-0 z-0">
        <Image
          src="/hero-bg.jpg"
          alt="Natural background"
          fill
          style={{ objectFit: "cover" }}
          priority
        />
        <div className="absolute inset-0 bg-gradient-to-r from-black/50 to-transparent" />
      </div>

      {/* Content */}
      <div className="container mx-auto px-4 md:px-6 z-10">
        <div className="max-w-2xl">
          <motion.h1 
            className="text-4xl md:text-6xl font-display font-bold text-white mb-6"
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.7 }}
          >
            Restore Your Body's Natural Balance
          </motion.h1>
          
          <motion.p 
            className="text-xl text-white/90 mb-8"
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.7, delay: 0.2 }}
          >
            Our natural cleansing formula is designed to support your body's detoxification process, boosting energy and overall wellbeing.
          </motion.p>
          
          <motion.div 
            className="flex flex-col sm:flex-row gap-4"
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.7, delay: 0.4 }}
          >
            <Button 
              as={Link}
              href="/products" 
              color="primary" 
              size="lg" 
              radius="full"
              className="font-medium text-lg hover-lift"
            >
              Shop Now
            </Button>
            
            <Button 
              as={Link}
              href="/about" 
              variant="bordered" 
              size="lg" 
              radius="full"
              className="font-medium text-lg text-white border-white hover-lift"
            >
              Learn More
            </Button>
          </motion.div>
        </div>
      </div>
    </section>
  );
};
```

```typescript
// File: components/ProductCard.tsx
"use client";

import { FC } from "react";
import { Card, CardBody, CardFooter, Button, Image } from "@nextui-org/react";
import Link from "next/link";
import { motion } from "framer-motion";

interface ProductCardProps {
  id: string;
  name: string;
  price: number;
  image: string;
  rating: number;
  reviewCount: number;
}

export const ProductCard: FC<ProductCardProps> = ({ 
  id, 
  name, 
  price, 
  image, 
  rating,
  reviewCount
}) => {
  return (
    <motion.div
      whileHover={{ y: -8 }}
      transition={{ type: "spring", stiffness: 300 }}
    >
      <Card 
        isPressable 
        className="overflow-hidden border border-gray-200 hover:border-primary/30 transition-all duration-300"
        as={Link}
        href={`/products/${id}`}
      >
        <CardBody className="p-0 overflow-hidden">
          <div className="relative h-64 w-full overflow-hidden group">
            <Image
              alt={name}
              className="object-cover w-full h-full group-hover:scale-105 transition-transform duration-500"
              src={image}
              width={300}
              height={400}
            />
            <div className="absolute bottom-0 left-0 w-full h-1/3 bg-gradient-to-t from-black/30 to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300" />
          </div>
        </CardBody>
        <CardFooter className="flex flex-col items-start p-4">
          <div className="flex items-center mb-1 space-x-1">
            {Array.from({ length: 5 }).map((_, i) => (
              <svg 
                key={i}
                className={`w-4 h-4 ${i < rating ? "text-yellow-400" : "text-gray-300"}`}
                xmlns="http://www.w3.org/2000/svg" 
                viewBox="0 0 24 24" 
                fill="currentColor"
              >
                <path d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14 2 9.27l6.91-1.01L12 2z" />
              </svg>
            ))}
            <span className="text-xs text-gray-500">({reviewCount})</span>
          </div>
          <h3 className="text-lg font-medium text-foreground">{name}</h3>
          <p className="font-bold text-primary text-xl mt-1">${price.toFixed(2)}</p>
          <Button 
            color="primary" 
            radius="full" 
            className="mt-3 w-full hover-scale" 
            onClick={(e) => {
              e.preventDefault();
              console.log(`Add ${name} to cart`);
            }}
          >
            Add to Cart
          </Button>
        </CardFooter>
      </Card>
    </motion.div>
  );
};
```

```typescript
// File: components/FeaturedProducts.tsx
"use client";

import { useState } from "react";
import { ProductCard } from "./ProductCard";
import { Tabs, Tab } from "@nextui-org/react";
import { motion } from "framer-motion";

const mockProducts = [
  {
    id: "cleanse-original",
    name: "Original Cleanse Formula",
    price: 39.99,
    image: "/products/original-cleanse.jpg",
    rating: 4.8,
    reviewCount: 124,
    category: "bestsellers"
  },
  {
    id: "cleanse-plus",
    name: "Balance Cleanse Plus",
    price: 49.99,
    image: "/products/cleanse-plus.jpg",
    rating: 4.9,
    reviewCount: 86,
    category: "bestsellers"
  },
  {
    id: "detox-tea",
    name: "Herbal Detox Tea",
    price: 24.99,
    image: "/products/detox-tea.jpg",
    rating: 4.6,
    reviewCount: 52,
    category: "new"
  },
  {
    id: "daily-supplement",
    name: "Daily Wellness Supplement",
    price: 29.99,
    image: "/products/daily-supplement.jpg",
    rating: 4.7,
    reviewCount: 73,
    category: "new"
  }
];

export const FeaturedProducts = () => {
  const [selectedCategory, setSelectedCategory] = useState<string>("bestsellers");
  
  const filteredProducts = mockProducts.filter(
    product => product.category === selectedCategory
  );
  
  return (
    <section className="py-12 px-4 md:px-6 container mx-auto">
      <div className="text-center mb-12">
        <motion.h2 
          className="text-3xl md:text-4xl font-display font-bold mb-4"
          initial={{ opacity: 0, y: 20 }}
          whileInView={{ opacity: 1, y: 0 }}
          viewport={{ once: true }}
          transition={{ duration: 0.5 }}
        >
          Our Products
        </motion.h2>
        <motion.p 
          className="text-lg text-gray-600 max-w-2xl mx-auto"
          initial={{ opacity: 0, y: 20 }}
          whileInView={{ opacity: 1, y: 0 }}
          viewport={{ once: true }}
          transition={{ duration: 0.5, delay: 0.2 }}
        >
          Discover our range of natural cleansing and wellness products
        </motion.p>
      </div>
      
      <Tabs 
        selectedKey={selectedCategory}
        onSelectionChange={(key) => setSelectedCategory(key as string)}
        variant="underlined"
        color="primary"
        classNames={{
          base: "justify-center mb-8",
          tabList: "gap-6",
          cursor: "bg-primary",
          tab: "max-w-fit px-2 h-10 text-medium",
        }}
      >
        <Tab key="bestsellers" title="Best Sellers" />
        <Tab key="new" title="New Arrivals" />
      </Tabs>
      
      <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
        {filteredProducts.map((product, index) => (
          <motion.div
            key={product.id}
            initial={{ opacity: 0, y: 20 }}
            whileInView={{ opacity: 1, y: 0 }}
            viewport={{ once: true }}
            transition={{ duration: 0.5, delay: 0.1 * index }}
          >
            <ProductCard {...product} />
          </motion.div>
        ))}
      </div>
    </section>
  );
};
```

```typescript
// File: components/BenefitsSection.tsx
"use client";

import { Card, CardBody } from "@nextui-org/react";
import { motion } from "framer-motion";
import { FaLeaf, FaFlask, FaBolt, FaHeart } from "react-icons/fa";

const benefits = [
  {
    title: "100% Natural",
    description: "Made with all-natural ingredients, free from additives and artificial substances.",
    icon: FaLeaf,
    color: "bg-green-100 text-green-600"
  },
  {
    title: "Scientifically Backed",
    description: "Our formula is backed by research and developed by health professionals.",
    icon: FaFlask,
    color: "bg-blue-100 text-blue-600"
  },
  {
    title: "Energy Boost",
    description: "Feel more energized and vibrant as your body releases toxins.",
    icon: FaBolt,
    color: "bg-yellow-100 text-yellow-600"
  },
  {
    title: "Wellness Support",
    description: "Support your overall wellness journey with our balanced approach.",
    icon: FaHeart,
    color: "bg-red-100 text-red-600"
  }
];

export const BenefitsSection = () => {
  return (
    <section className="py-16 bg-secondary">
      <div className="container mx-auto px-4 md:px-6">
        <div className="text-center mb-12">
          <motion.h2 
            className="text-3xl md:text-4xl font-display font-bold mb-4"
            initial={{ opacity: 0, y: 20 }}
            whileInView={{ opacity: 1, y: 0 }}
            viewport={{ once: true }}
            transition={{ duration: 0.5 }}
          >
            The Balance Cleanse Difference
          </motion.h2>
          <motion.p 
            className="text-lg text-gray-600 max-w-2xl mx-auto"
            initial={{ opacity: 0, y: 20 }}
            whileInView={{ opacity: 1, y: 0 }}
            viewport={{ once: true }}
            transition={{ duration: 0.5, delay: 0.2 }}
          >
            Why our customers choose Balance Cleanse for their wellness journey
          </motion.p>
        </div>

        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
          {benefits.map((benefit, index) => {
            const Icon = benefit.icon;
            
            return (
              <motion.div
                key={benefit.title}
                initial={{ opacity: 0, y: 20 }}
                whileInView={{ opacity: 1, y: 0 }}
                viewport={{ once: true }}
                transition={{ duration: 0.5, delay: 0.1 * index }}
              >
                <Card className="border-none hover-lift cursor-default">
                  <CardBody className="flex flex-col items-center text-center p-6">
                    <div className={`w-14 h-14 ${benefit.color} rounded-full flex items-center justify-center mb-4`}>
                      <Icon className="text-xl" />
                    </div>
                    <h3 className="text-xl font-bold mb-2">{benefit.title}</h3>
                    <p className="text-gray-600">{benefit.description}</p>
                  </CardBody>
                </Card>
              </motion.div>
            );
          })}
        </div>
      </div>
    </section>
  );
};
```

```typescript
// File: components/TestimonialsSection.tsx
"use client";

import { Card, CardBody, Avatar, Pagination } from "@nextui-org/react";
import { useState } from "react";
import { motion } from "framer-motion";
import { FaQuoteLeft } from "react-icons/fa";

const testimonials = [
  {
    id: 1,
    name: "Sarah Johnson",
    title: "Verified Customer",
    avatar: "/testimonials/sarah.jpg",
    quote: "Balance Cleanse has been a game-changer for my wellness routine. After just two weeks, I noticed improved energy levels and better digestion. It's now a staple in my health regimen!"
  },
  {
    id: 2,
    name: "Michael Chen",
    title: "Wellness Coach",
    avatar: "/testimonials/michael.jpg",
    quote: "As a wellness coach, I'm very selective about the products I recommend. Balance Cleanse stands out for its natural ingredients and effective results. My clients consistently report positive outcomes."
  },
  {
    id: 3,
    name: "Jessica Miller",
    title: "Yoga Instructor",
    avatar: "/testimonials/jessica.jpg",
    quote: "I've incorporated Balance Cleanse into my post-yoga routine, and the difference is noticeable. My body feels lighter, and my mind clearer. It complements my practice perfectly."
  },
  {
    id: 4,
    name: "David Wilson",
    title: "Fitness Enthusiast",
    avatar: "/testimonials/david.jpg",
    quote: "After years of trying different supplements, Balance Cleanse delivers what it promises. My recovery times have improved, and I feel more energized during my workouts."
  },
  {
    id: 5,
    name: "Emily Rodriguez",
    title: "Nutritionist",
    avatar: "/testimonials/emily.jpg",
    quote: "The science behind Balance Cleanse aligns with what I recommend to my nutrition clients. It supports the body's natural detoxification process without harsh ingredients."
  },
  {
    id: 6,
    name: "James Taylor",
    title: "Verified Customer",
    avatar: "/testimonials/james.jpg",
    quote: "After struggling with low energy for years, Balance Cleanse has helped me feel like myself again. I appreciate the natural approach and sustainable results."
  }
];

export const TestimonialsSection = () => {
  const [currentPage, setCurrentPage] = useState(1);
  const testimonialsPerPage = 3;
  
  const indexOfLastTestimonial = currentPage * testimonialsPerPage;
  const indexOfFirstTestimonial = indexOfLastTestimonial - testimonialsPerPage;
  const currentTestimonials = testimonials.slice(indexOfFirstTestimonial, indexOfLastTestimonial);
  
  return (
    <section className="py-16 px-4 md:px-6 container mx-auto">
      <div className="text-center mb-12">
        <motion.h2 
          className="text-3xl md:text-4xl font-display font-bold mb-4"
          initial={{ opacity: 0, y: 20 }}
          whileInView={{ opacity: 1, y: 0 }}
          viewport={{ once: true }}
          transition={{ duration: 0.5 }}
        >
          What Our Customers Say
        </motion.h2>
        <motion.p 
          className="text-lg text-gray-600 max-w-2xl mx-auto"
          initial={{ opacity: 0, y: 20 }}
          whileInView={{ opacity: 1, y: 0 }}
          viewport={{ once: true }}
          transition={{ duration: 0.5, delay: 0.2 }}
        >
          Discover the experiences of those who have made Balance Cleanse part of their wellness journey
        </motion.p>
      </div>
      
      <div className="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
        {currentTestimonials.map((testimonial, index) => (
          <motion.div
            key={testimonial.id}
            initial={{ opacity: 0, y: 20 }}
            whileInView={{ opacity: 1, y: 0 }}
            viewport={{ once: true }}
            transition={{ duration: 0.5, delay: 0.1 * index }}
          >
            <Card className="h-full hover-lift">
              <CardBody className="p-6">
                <FaQuoteLeft className="text-primary/20 text-4xl mb-4" />
                <p className="text-gray-700 italic mb-6">{testimonial.quote}</p>
                <div className="flex items-center">
                  <Avatar 
                    src={testimonial.avatar} 
                    className="mr-4"
                    size="lg"
                  />
                  <div>
                    <h4 className="font-bold">{testimonial.name}</h4>
                    <p className="text-gray-500 text-sm">{testimonial.title}</p>
                  </div>
                </div>
              </CardBody>
            </Card>
          </motion.div>
        ))}
      </div>
      
      <div className="flex justify-center">
        <Pagination
          total={Math.ceil(testimonials.length / testimonialsPerPage)}
          initialPage={1}
          color="primary"
          onChange={(page) => setCurrentPage(page)}
        />
      </div>
    </section>
  );
};
```

```typescript
// File: components/NewsletterSignup.tsx
"use client";

import { useState } from "react";
import { Input, Button } from "@nextui-org/react";
import { motion } from "framer-motion";

export const NewsletterSignup = () => {
  const [email, setEmail] = useState("");
  const [isSubmitted, setIsSubmitted] = useState(false);
  
  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    if (email) {
      // In a real application, you would send this to your backend
      console.log("Email submitted:", email);
      setIsSubmitted(true);
      setEmail("");
      
      // Reset the submitted state after 3 seconds
      setTimeout(() => {
        setIsSubmitted(false);
      }, 3000);
    }
  };
  
  return (
    <section className="py-16 bg-primary text-white">
      <div className="container mx-auto px-4 md:px-6">
        <div className="max-w-3xl mx-auto text-center">
          <motion.h2 
            className="text-3xl md:text-4xl font-display font-bold mb-4"
            initial={{ opacity: 0, y: 20 }}
            whileInView={{ opacity: 1, y: 0 }}
            viewport={{ once: true }}
            transition={{ duration: 0.5 }}
          >
            Join Our Wellness Community
          </motion.h2>
          <motion.p 
            className="text-lg text-white/80 mb-8"
            initial={{ opacity: 0, y: 20 }}
            whileInView={{ opacity: 1, y: 0 }}
            viewport={{ once: true }}
            transition={{ duration: 0.5, delay: 0.2 }}
          >
            Subscribe to our newsletter for wellness tips, special offers, and updates on new products
          </motion.p>
          
          <motion.form 
            onSubmit={handleSubmit}
            className="flex flex-col sm:flex-row gap-4 max-w-lg mx-auto"
            initial={{ opacity: 0, y: 20 }}
            whileInView={{ opacity: 1, y: 0 }}
            viewport={{ once: true }}
            transition={{ duration: 0.5, delay: 0.3 }}
          >
            <Input
              type="email"
              label="Email address"
              value={email}
              onChange={(e) => setEmail(e.target.value)}
              placeholder="Enter your email"
              variant="bordered"
              radius="full"
              classNames={{
                input: "text-white",
                label: "text-white/80",
                inputWrapper: "border-white/30 hover:border-white data-[hover=true]:border-white group-data-[focus=true]:border-white"
              }}
              className="flex-grow"
              required
            />
            <Button 
              type="submit" 
              color="secondary" 
              radius="full"
              className="font-medium text-primary hover-lift"
              isLoading={isSubmitted}
            >
              {isSubmitted ? "Subscribed!" : "Subscribe"}
            </Button>
          </motion.form>
        </div>
      </div>
    </section>
  );
};
```

```typescript
// File: components/Footer.tsx
import Link from "next/link";
import Image from "next/image";
import { FaFacebook, FaTwitter, FaInstagram, FaPinterest } from "react-icons/fa";

export const Footer = () => {
  return (
    <footer className="bg-white border-t border-gray-200">
      <div className="container mx-auto px-4 py-12 md:py-16">
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-8">
          <div>
            <Link href="/" className="flex items-center gap-2 mb-4">
              <Image 
                src="/logo.svg" 
                alt="Balance Cleanse Logo" 
                width={32} 
                height={32} 
              />
              <span className="font-display font-bold text-primary text-lg">
                Balance Cleanse
              </span>
            </Link>
            <p className="text-gray-600 mb-4">
              Natural solutions for a balanced, healthy lifestyle through premium cleansing products.
            </p>
            <div className="flex space-x-4">
              <a href="#" className="text-gray-400 hover:text-primary transition-colors">
                <FaFacebook size={20} />
              </a>
              <a href="#" className="text-gray-400 hover:text-primary transition-colors">
                <FaTwitter size={20} />
              </a>
              <a href="#" className="text-gray-400 hover:text-primary transition-colors">
                <FaInstagram size={20} />
              </a>
              <a href="#" className="text-gray-400 hover:text-primary transition-colors">
                <FaPinterest size={20} />
              </a>
            </div>
          </div>
          
          <div>
            <h3 className="font-bold text-lg mb-4">Shop</h3>
            <ul className="space-y-2">
              <li>
                <Link href="/products" className="text-gray-600 hover:text-primary transition-colors">
                  All Products
                </Link>
              </li>
              <li>
                <Link href="/products/bestsellers" className="text-gray-600 hover:text-primary transition-colors">
                  Best Sellers
                </Link>
              </li>
              <li>
                <Link href="/products/new" className="text-gray-600 hover:text-primary transition-colors">
                  New Arrivals
                </Link>
              </li>
              <li>
                <Link href="/products/bundles" className="text-gray-600 hover:text-primary transition-colors">
                  Bundles & Kits
                </Link>
              </li>
            </ul>
          </div>
          
          <div>
            <h3 className="font-bold text-lg mb-4">About</h3>
            <ul className="space-y-2">
              <li>
                <Link href="/about" className="text-gray-600 hover:text-primary transition-colors">
                  Our Story
                </Link>
              </li>
              <li>
                <Link href="/ingredients" className="text-gray-600 hover:text-primary transition-colors">
                  Ingredients
                </Link>
              </li>
              <li>
                <Link href="/blog" className="text-gray-600 hover:text-primary transition-colors">
                  Blog
                </Link>
              </li>
              <li>
                <Link href="/testimonials" className="text-gray-600 hover:text-primary transition-colors">
                  Testimonials
                </Link>
              </li>
            </ul>
          </div>
          
          <div>
            <h3 className="font-bold text-lg mb-4">Customer Service</h3>
            <ul className="space-y-2">
              <li>
                <Link href="/contact" className="text-gray-600 hover:text-primary transition-colors">
                  Contact Us
                </Link>
              </li>
              <li>
                <Link href="/faq" className="text-gray-600 hover:text-primary transition-colors">
                  FAQs
                </Link>
              </li>
              <li>
                <Link href="/shipping" className="text-gray-600 hover:text-primary transition-colors">
                  Shipping & Returns
                </Link>
              </li>
              <li>
                <Link href="/privacy" className="text-gray-600 hover:text-primary transition-colors">
                  Privacy Policy
                </Link>
              </li>
            </ul>
          </div>
        </div>
        
        <div className="border-t border-gray-200 mt-12 pt-8 text-center text-gray-500 text-sm">
          <p>© {new Date().getFullYear()} Balance Cleanse. All rights reserved.</p>
        </div>
      </div>
    </footer>
  );
};
```

Would you like me to explain or break down the code structure?

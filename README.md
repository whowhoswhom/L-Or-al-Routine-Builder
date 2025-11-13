# L'Oréal Routine Builder

An AI-powered skincare routine builder that helps users create personalized skincare routines using L'Oréal and CeraVe products.

## 🌟 Features

### Core Functionality
- **Product Selection**: Browse and select from 12 professional skincare products
- **AI-Powered Routines**: Generate personalized morning and evening skincare routines using OpenAI's GPT-4
- **Interactive Chat**: Ask follow-up questions about your routine with full conversation memory
- **Persistent Storage**: Selected products are saved to localStorage and persist across sessions
- **Product Details**: View detailed descriptions for each product with expandable cards
- **Secure API Integration**: All API calls routed through Cloudflare Workers to protect API keys

### Advanced Features
- **Product Search**: Real-time search filtering by name, brand, or keyword
- **Category Filters**: Filter products by type (Cleansers, Moisturizers, Serums, etc.)
- **RTL Language Support**: Full right-to-left layout support for Arabic and Hebrew
- **Responsive Design**: Optimized for desktop, tablet, and mobile devices
- **L'Oréal Branding**: Professional styling following L'Oréal's brand guidelines

## 🚀 Live Demo

[View Live Site](https://whowhoswhom.github.io/loreal-routine-builder/)

## 🛠️ Technologies Used

- **HTML5** - Semantic markup
- **CSS3** - Custom styling with CSS Grid and Flexbox
- **Vanilla JavaScript** - No frameworks required
- **OpenAI API** - GPT-4-mini for AI-powered recommendations
- **Cloudflare Workers** - Secure API proxy
- **localStorage** - Client-side data persistence

## 📋 Prerequisites

To run this project locally, you need:

1. **OpenAI API Key** - Get one at [platform.openai.com](https://platform.openai.com/)
2. **Cloudflare Account** - Free account at [dash.cloudflare.com](https://dash.cloudflare.com/)

## ⚙️ Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/whowhoswhom/loreal-routine-builder.git
cd loreal-routine-builder
```

### 2. Set Up Cloudflare Worker

1. Log in to your [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. Navigate to **Workers & Pages**
3. Click **Create Application** → **Create Worker**
4. Name your worker (e.g., `loreal-chatbot-proxy`)
5. Click **Deploy** then **Edit Code**
6. Copy the worker code from `cloudflare-worker.js` (see below)
7. Click **Save and Deploy**

**Cloudflare Worker Code:**

```javascript
export default {
  async fetch(request, env) {
    // Only allow POST requests
    if (request.method !== 'POST') {
      return new Response('Method not allowed', { status: 405 });
    }

    // Add CORS headers
    const corsHeaders = {
      'Access-Control-Allow-Origin': '*',
      'Access-Control-Allow-Methods': 'POST, OPTIONS',
      'Access-Control-Allow-Headers': 'Content-Type',
    };

    // Handle preflight requests
    if (request.method === 'OPTIONS') {
      return new Response(null, { headers: corsHeaders });
    }

    try {
      // Get the request body
      const body = await request.json();

      // Forward to OpenAI
      const response = await fetch('https://api.openai.com/v1/chat/completions', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Authorization': `Bearer ${env.OPENAI_API_KEY}`
        },
        body: JSON.stringify(body)
      });

      const data = await response.json();

      return new Response(JSON.stringify(data), {
        headers: {
          'Content-Type': 'application/json',
          ...corsHeaders
        }
      });
    } catch (error) {
      return new Response(JSON.stringify({ error: error.message }), {
        status: 500,
        headers: {
          'Content-Type': 'application/json',
          ...corsHeaders
        }
      });
    }
  }
};
```

### 3. Add Environment Variable

1. In your Cloudflare Worker, go to **Settings** → **Variables and Secrets**
2. Click **Add variable**
3. Name: `OPENAI_API_KEY`
4. Value: Your OpenAI API key
5. Click **Encrypt** then **Save**

### 4. Update Worker URL

In `index.html`, update line 636 with your Cloudflare Worker URL:

```javascript
const CLOUDFLARE_WORKER_URL = 'https://your-worker-name.your-subdomain.workers.dev';
```

### 5. Deploy to GitHub Pages

1. Push your code to GitHub
2. Go to **Settings** → **Pages**
3. Select **Source**: Deploy from branch
4. Select **Branch**: main
5. Click **Save**

Your site will be live at: `https://yourusername.github.io/loreal-routine-builder/`

## 📱 Usage

1. **Browse Products**: View all available skincare products with images and descriptions
2. **Filter & Search**: Use category filters or search bar to find specific products
3. **Select Products**: Click on products to add them to your selection
4. **View Details**: Click "Show Description" to read full product information
5. **Generate Routine**: Click the "Generate Personalized Routine" button
6. **Chat**: Ask follow-up questions about your routine, product order, or skincare advice
7. **Manage Selection**: Remove individual products or clear all selections

## 🎨 Product Categories

- **Cleansers**: Gentle and foaming facial cleansers
- **Moisturizers**: Hydrating creams and lotions
- **Serums**: Targeted treatments (Vitamin C, Retinol, Hyaluronic Acid)
- **Treatments**: Acne solutions and resurfacing products
- **Sunscreen**: SPF protection for daily use

## 🔒 Security

- ✅ API keys never exposed in client-side code
- ✅ All API requests proxied through Cloudflare Workers
- ✅ Environment variables encrypted in Cloudflare
- ✅ CORS properly configured for security

## 🌐 Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)
- Mobile browsers (iOS Safari, Chrome Mobile)

## 📄 License

This project was created as part of the Global Career Accelerator program at the University of Louisville.

## 👤 Author

**Jose Fuentes**
- GitHub: [@whowhoswhom](https://github.com/whowhoswhom)
- LinkedIn: [Jose Fuentes](https://www.linkedin.com/in/jose-fuentes-software/)
- Email: josefuentes2013.jf@gmail.com

## 🙏 Acknowledgments

- University of Louisville - Global Career Accelerator Program
- L'Oréal - Brand guidelines and product information
- OpenAI - AI-powered recommendations
- Cloudflare - Secure API proxy hosting

## 📝 Project Status

**Completed** - November 2025

Developed as Project 5 for the GCA Web Development Track, demonstrating advanced JavaScript, API integration, and AI implementation skills.

---

**Note**: This is an educational project and not affiliated with or endorsed by L'Oréal. Product information is used for educational purposes only.

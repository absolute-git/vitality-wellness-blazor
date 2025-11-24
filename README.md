# Vitality Wellness Center

A professional, modern website for a wellness center built with Blazor WebAssembly (Static).

![.NET](https://img.shields.io/badge/.NET-8.0-512BD4)
![Blazor](https://img.shields.io/badge/Blazor-WebAssembly-512BD4)
![License](https://img.shields.io/badge/License-MIT-green)

## Overview

Vitality Wellness Center is a fully responsive, static website designed for wellness businesses offering physical therapy, personal training, massage therapy, and yoga services. Built with Blazor WebAssembly, it can be deployed to any static hosting service.

## Features

- **Modern Design** - Clean, professional look with a calming green wellness color palette
- **Fully Responsive** - Mobile-first design that works on all devices
- **Fast Loading** - Static site generation for optimal performance
- **SEO Friendly** - Semantic HTML structure
- **No Backend Required** - Can be hosted on GitHub Pages, Netlify, Azure Static Web Apps, etc.

## Pages

| Page | Description |
|------|-------------|
| **Home** | Hero section with stats, features, service showcase, testimonials, and CTA |
| **About** | Company story, core values, team members, and facility gallery |
| **Services** | Service cards with images, process steps, and pricing plans |
| **Contact** | Booking form with service selection and contact information |

## Tech Stack

- [.NET 8](https://dotnet.microsoft.com/)
- [Blazor WebAssembly](https://docs.microsoft.com/aspnet/core/blazor/)
- [Bootstrap](https://getbootstrap.com/) (grid system)
- Custom CSS with CSS Variables
- [Inter Font](https://fonts.google.com/specimen/Inter)
- [Unsplash](https://unsplash.com/) (stock images)

## Getting Started

### Prerequisites

- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)

### Installation

1. Clone the repository
   ```bash
   git clone https://github.com/absolute-git/vitality-wellness-blazor.git
   cd vitality-wellness-blazor
   ```

2. Restore dependencies
   ```bash
   dotnet restore
   ```

3. Run the development server
   ```bash
   dotnet run
   ```

4. Open your browser to `http://localhost:5000`

### Building for Production

```bash
dotnet publish -c Release
```

The published static files will be in `bin/Release/net8.0/publish/wwwroot/`

## Deployment

This static site can be deployed to:

- **GitHub Pages** - Push to `gh-pages` branch
- **Netlify** - Connect repo and set publish directory to `bin/Release/net8.0/publish/wwwroot`
- **Azure Static Web Apps** - Use the Blazor preset
- **Vercel** - Configure output directory
- **Any static host** - Just upload the `wwwroot` folder contents

## Customization

### Colors

Edit the CSS variables in `wwwroot/css/app.css`:

```css
:root {
    --primary-color: #10b981;    /* Main brand color */
    --primary-dark: #059669;     /* Darker shade */
    --primary-light: #34d399;    /* Lighter shade */
    --secondary-color: #1e3a5f;  /* Dark blue */
    --accent-color: #0891b2;     /* Accent cyan */
}
```

### Content

- Update text in `Pages/*.razor` files
- Replace images by changing Unsplash URLs or adding local images to `wwwroot/images/`
- Modify contact info in `Layout/MainLayout.razor` (footer) and `Pages/Contact.razor`

## Project Structure

```
MyWebSite/
├── Layout/
│   ├── MainLayout.razor      # Main layout with header/footer
│   └── NavMenu.razor         # Navigation component
├── Pages/
│   ├── Home.razor            # Home page
│   ├── About.razor           # About page
│   ├── Services.razor        # Services page
│   └── Contact.razor         # Contact page
├── wwwroot/
│   ├── css/
│   │   └── app.css           # Main stylesheet
│   └── index.html            # HTML entry point
├── App.razor                 # App component
├── Program.cs                # Application entry
└── MyWebSite.csproj          # Project file
```

## License

This project is open source and available under the [MIT License](LICENSE).

## Acknowledgments

- Images from [Unsplash](https://unsplash.com/)
- Icons are inline SVGs based on [Feather Icons](https://feathericons.com/)

# GSAP Animation Portfolio

A showcase of modern web animations built with Vue.js, GSAP, and Tailwind CSS. This project demonstrates various animation techniques that can be used in web applications.

## 🚀 Features

- **Text Reveal Animations** - Staggered text animations with 3D rotation effects
- **Interactive Button Effects** - Magnetic, ripple, glitch, and morph hover animations
- **3D Card Flip Animations** - Smooth card flip effects with backface visibility
- **Loading Animations** - Various loading spinners and progress indicators
- **Scroll-Triggered Animations** - Parallax effects and scroll-based animations

## 🛠️ Tech Stack

- **Vue.js 3** - Progressive JavaScript framework
- **GSAP (GreenSock)** - Professional-grade animation library
- **Tailwind CSS** - Utility-first CSS framework
- **Vite** - Fast build tool and development server

## 📦 Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd gasp-animation
```

2. Install dependencies:
```bash
npm install
```

3. Start the development server:
```bash
npm run dev
```

4. Open your browser and navigate to `http://localhost:5173`

## 🎨 Animation Demos

### 1. Text Reveal Animation
- Staggered letter animations
- 3D rotation effects
- Replay functionality
- Smooth easing curves

### 2. Button Hover Effects
- **Magnetic Effect**: Scale and background morph
- **Ripple Effect**: Click-triggered ripple animation
- **Glitch Effect**: Shake and scale animation
- **Morph Effect**: Border radius and color transition

### 3. Card Flip Animation
- 3D flip effects with proper backface visibility
- Smooth rotation animations
- Interactive click-to-flip functionality
- Reset functionality

### 4. Loading Animations
- **Spinner**: Continuous rotation animation
- **Bouncing Dots**: Staggered vertical movement
- **Pulse**: Scale and opacity animation
- **Morphing Bars**: Rotation and border radius changes

### 5. Scroll-Triggered Animations
- Parallax background effects
- Scroll progress indicator
- Staggered element animations
- Scale and rotation effects on scroll

## 🔧 Customization

### Adding New Animations

1. Create a new Vue component in `src/components/animations/`
2. Import GSAP and implement your animation logic
3. Add the component to the `demos` array in `AnimationDemos.vue`
4. Update the navigation and component imports

### Modifying Existing Animations

Each animation component is self-contained and can be easily modified:
- Adjust timing and easing in the GSAP animations
- Modify colors and styling using Tailwind classes
- Add new interactive elements
- Customize animation parameters

## 📱 Responsive Design

All animations are fully responsive and work on:
- Desktop computers
- Tablets
- Mobile devices

## 🎯 Performance Optimization

- GPU-accelerated animations using `transform` properties
- Efficient GSAP tweens and timelines
- Optimized scroll triggers
- Smooth 60fps animations

## 🚀 Deployment

Build the project for production:
```bash
npm run build
```

The built files will be in the `dist/` directory, ready for deployment.

## 📚 Learning Resources

- [GSAP Documentation](https://greensock.com/docs/)
- [Vue.js Guide](https://vuejs.org/guide/)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)

## 🤝 Contributing

Feel free to contribute by:
- Adding new animation demos
- Improving existing animations
- Fixing bugs
- Enhancing documentation

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

**Created for interview showcase** - Demonstrating modern web animation techniques with Vue.js and GSAP.

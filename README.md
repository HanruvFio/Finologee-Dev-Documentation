# Finologee-Dev-Documentation

## Page width
- 1448 boxed

## Paddings for sections
- Desktop 4 Tablet: 75, 30, 75, 30
- Mobile: 40, 20, 40, 20

## Coontainer content gaps
- 20px

## Btns Sizing/Radius
- Corner radius: 100
- Padding: 10, 20, 10, 20
### Btn Groups spacomg
- 10 10
## Breadcrumb margins
- Desktop 4 Tablet: 40 bottom
- Mobile: 20 bottom

# Animations  
## Bento Boxes  
- Icons > Fade In Normal
- Hover Effect on Box > Scale: 1.01
- Box Entrance Animation > Fade in Up (Fast) staggered with 100ms for each block

## Flip Card Carousel 
- On Widget Slide in Right (normal)

## Trusted By Logos 
- Fade In (normal) > Offset 50ms
# About Image
- Slide in Right (normal)

# View All implementation

## Component Structure
To ensure the "View All" button correctly pushes down as content expands, widgets must be ordered as follows in the Elementor Navigator:

Main Container: (Parent wrapper)

Sub-Container (Always Visible): Contains the header "Banking Orchestrator 6.4" and the first three release items.

Sub-Container (#my-hidden-content): Contains the remaining four release items.

Icon List (#btn-view-all): Configured with the Plus (+) SVG and "VIEW ALL" text.

Icon List (#btn-view-less): Configured with the Flat Line (-) SVG and "VIEW LESS" text.

## CSS Implementation
Add this to the Custom CSS area (Page Settings or the Hidden Container's Advanced tab). This handles the smooth "Accordion" reveal and the dual-speed transition.

```
/* --- Hidden Content Container --- */
#my-hidden-content {
    max-height: 0;
    overflow: hidden;
    opacity: 0;
    /* CLOSING SPEED: 1.2s for an elegant, intentional collapse */
    transition: max-height 1.2s cubic-bezier(0.4, 0, 0.2, 1), 
                opacity 0.8s ease;
}

#my-hidden-content.is-open {
    max-height: 2000px; /* High value to ensure all content is revealed */
    opacity: 1;
    /* OPENING SPEED: 0.5s for a snappy, responsive feel */
    transition: max-height 0.5s ease-in-out, 
                opacity 0.5s ease-in-out;
}

/* --- Toggle Buttons --- */
#btn-view-all, #btn-view-less {
    user-select: none; /* Prevents blue text highlighting on click */
    cursor: pointer;
}

#btn-view-less {
    display: none; /* Hidden on initial page load */
}
```
## JavaScript Logic
Place this code in an HTML Widget to manage the state toggling and the automated scroll-back behavior.

```
<script>
document.addEventListener('DOMContentLoaded', function() {
    const btnAll = document.getElementById('btn-view-all');
    const btnLess = document.getElementById('btn-view-less');
    const content = document.getElementById('my-hidden-content');

    // --- EXPAND ACTION ---
    btnAll.addEventListener('click', function(e) {
        e.preventDefault();
        
        content.classList.add('is-open'); // Triggers 0.5s reveal
        
        btnAll.style.display = "none";    // Swap to View Less button
        btnLess.style.display = "flex";   
    });

    // --- COLLAPSE ACTION ---
    btnLess.addEventListener('click', function(e) {
        e.preventDefault();
        
        content.classList.remove('is-open'); // Triggers 1.2s slow collapse
        
        btnAll.style.display = "flex";    // Swap to View All button
        btnLess.style.display = "none";   
        
        /* Delay the scroll-back to match the 1.2s animation,
           ensuring the user doesn't lose their place while 
           the container is still shrinking.
        */
        setTimeout(() => {
            btnAll.scrollIntoView({ 
                behavior: 'smooth', 
                block: 'end' 
            });
        }, 1200); 
    });
});
</script>
```

## Key Technical Highlights
Asymmetrical Animation: Optimizes UX by providing instant feedback on expansion (0.5s) while maintaining a high-end feel during closure (1.2s).

Staging-Safe Asset Management: Uses two distinct Icon List widgets to swap SVGs, bypassing potential broken file path issues common with staging-to-production migrations.

Auto-Scroll Anchor: The script calculates the precise end of the collapse animation before executing scrollIntoView, ensuring smooth page stabilization.

Text Interaction: Implements user-select: none to treat the "VIEW ALL" text as a UI button rather than selectable paragraph text.

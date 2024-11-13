<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Service Categories</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css" rel="stylesheet">
    <style>
        :root {
            --primary-color: #FFD700;
            --secondary-color: #FDB931;
            --background-dark: #1a1a1f;
            --text-light: #ffffff;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background-color: var(--background-dark);
            color: var(--text-light);
            font-family: -apple-system, system-ui, BlinkMacSystemFont, "Segoe UI", Roboto, Ubuntu;
        }

        .category-slider-container {
            position: relative;
            max-width: 100%;
            padding: 20px;
            overflow: hidden;
        }

        .category-grid {
            display: flex;
            transition: transform 0.3s ease;
            gap: 15px;
            padding: 10px 5px;
        }

        .category-page {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 15px;
            min-width: 100%;
            flex-shrink: 0;
        }

        .category-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 10px;
            padding: 15px 10px;
            background: rgba(255, 215, 0, 0.1);
            border: 1px solid rgba(255, 215, 0, 0.2);
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .category-item:hover {
            transform: translateY(-3px);
            background: rgba(255, 215, 0, 0.2);
        }

        .category-item i {
            font-size: 24px;
            color: var(--primary-color);
        }

        .category-item span {
            font-size: 14px;
            text-align: center;
            color: var(--text-light);
        }

        .slider-button {
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            background: rgba(255, 215, 0, 0.2);
            border: none;
            color: var(--primary-color);
            width: 40px;
            height: 40px;
            border-radius: 50%;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s ease;
            z-index: 10;
        }

        .slider-button:hover {
            background: rgba(255, 215, 0, 0.3);
        }

        .slider-button.prev {
            left: 10px;
        }

        .slider-button.next {
            right: 10px;
        }

        .slider-dots {
            display: flex;
            justify-content: center;
            gap: 8px;
            margin-top: 15px;
        }

        .dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background: rgba(255, 215, 0, 0.2);
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .dot.active {
            background: var(--primary-color);
            width: 24px;
            border-radius: 4px;
        }

        @media (max-width: 768px) {
            .category-page {
                grid-template-columns: repeat(2, 1fr);
            }
        }

        @media (max-width: 480px) {
            .slider-button {
                width: 30px;
                height: 30px;
                font-size: 14px;
            }
        }
    </style>
</head>
<body>
    <div class="category-slider-container">
        <button class="slider-button prev">
            <i class="fas fa-chevron-left"></i>
        </button>
        <button class="slider-button next">
            <i class="fas fa-chevron-right"></i>
        </button>
        
        <div class="category-grid" id="categoryGrid">
            <!-- Will be populated by JavaScript -->
        </div>
        
        <div class="slider-dots" id="sliderDots">
            <!-- Will be populated by JavaScript -->
        </div>
    </div>

    <script>
        const categories = [
            { id: 'plumber', name: 'Plumber', icon: 'fa-wrench' },
            { id: 'electrician', name: 'Electrician', icon: 'fa-bolt' },
            { id: 'mechanic', name: 'Mechanic', icon: 'fa-car' },
            { id: 'carpenter', name: 'Carpenter', icon: 'fa-hammer' },
            { id: 'painter', name: 'Painter', icon: 'fa-paint-roller' },
            { id: 'cleaner', name: 'Cleaner', icon: 'fa-broom' },
            { id: 'gardener', name: 'Gardener', icon: 'fa-leaf' },
            { id: 'ac_repair', name: 'AC Repair', icon: 'fa-snowflake' },
            { id: 'pest_control', name: 'Pest Control', icon: 'fa-bug' },
            { id: 'appliance', name: 'Appliance', icon: 'fa-blender' },
            { id: 'locksmith', name: 'Locksmith', icon: 'fa-key' },
            { id: 'moving', name: 'Moving', icon: 'fa-truck' },
            { id: 'security', name: 'Security', icon: 'fa-shield-alt' },
            { id: 'interior', name: 'Interior', icon: 'fa-couch' },
            { id: 'roof', name: 'Roofing', icon: 'fa-home' },
            { id: 'massage', name: 'Massage', icon: 'fa-spa' }
        ];

        const categoriesPerPage = 4;
        const totalPages = Math.ceil(categories.length / categoriesPerPage);
        let currentPage = 0;

        function createCategoryPages() {
            const categoryGrid = document.getElementById('categoryGrid');
            categoryGrid.innerHTML = '';

            for (let i = 0; i < totalPages; i++) {
                const page = document.createElement('div');
                page.className = 'category-page';
                
                const startIdx = i * categoriesPerPage;
                const endIdx = startIdx + categoriesPerPage;
                const pageCategories = categories.slice(startIdx, endIdx);

                pageCategories.forEach(category => {
                    const categoryItem = document.createElement('div');
                    categoryItem.className = 'category-item';
                    categoryItem.innerHTML = `
                        <i class="fas ${category.icon}"></i>
                        <span>${category.name}</span>
                    `;
                    categoryItem.addEventListener('click', () => handleCategorySelection(category.id));
                    page.appendChild(categoryItem);
                });

                categoryGrid.appendChild(page);
            }

            updateSliderPosition();
            createDots();
        }

        function createDots() {
            const dotsContainer = document.getElementById('sliderDots');
            dotsContainer.innerHTML = '';

            for (let i = 0; i < totalPages; i++) {
                const dot = document.createElement('div');
                dot.className = `dot ${i === currentPage ? 'active' : ''}`;
                dot.addEventListener('click', () => {
                    currentPage = i;
                    updateSliderPosition();
                    updateDots();
                });
                dotsContainer.appendChild(dot);
            }
        }

        function updateDots() {
            const dots = document.querySelectorAll('.dot');
            dots.forEach((dot, index) => {
                dot.className = `dot ${index === currentPage ? 'active' : ''}`;
            });
        }

        function updateSliderPosition() {
            const categoryGrid = document.getElementById('categoryGrid');
            categoryGrid.style.transform = `translateX(-${currentPage * 100}%)`;
        }

        function handleCategorySelection(categoryId) {
            console.log('Selected category:', categoryId);
            // Add your category selection logic here
        }

        // Initialize slider
        document.addEventListener('DOMContentLoaded', () => {
            createCategoryPages();

            // Add button event listeners
            document.querySelector('.prev').addEventListener('click', () => {
                if (currentPage > 0) {
                    currentPage--;
                    updateSliderPosition();
                    updateDots();
                }
            });

            document.querySelector('.next').addEventListener('click', () => {
                if (currentPage < totalPages - 1) {
                    currentPage++;
                    updateSliderPosition();
                    updateDots();
                }
            });
        });

        // Add touch support for mobile devices
        let touchStartX = 0;
        let touchEndX = 0;

        document.getElementById('categoryGrid').addEventListener('touchstart', e => {
            touchStartX = e.changedTouches[0].screenX;
        }, false);

        document.getElementById('categoryGrid').addEventListener('touchend', e => {
            touchEndX = e.changedTouches[0].screenX;
            handleSwipe();
        }, false);

        function handleSwipe() {
            const swipeThreshold = 50;
            const diff = touchStartX - touchEndX;

            if (Math.abs(diff) > swipeThreshold) {
                if (diff > 0 && currentPage < totalPages - 1) {
                    // Swipe left
                    currentPage++;
                } else if (diff < 0 && currentPage > 0) {
                    // Swipe right
                    currentPage--;
                }
                updateSliderPosition();
                updateDots();
            }
        }
    </script>
</body>
</html>

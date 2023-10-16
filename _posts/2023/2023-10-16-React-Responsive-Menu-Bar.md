---
layout: post
title: 'React學習筆記1 實作Responsive Menu Bar'
tags: ["React"]
date: 2023-10-16 17:00
comments: true
categories: 
- "學習筆記"
- "React"
---
# React實作Responsive Menu Bar
工作上遇到的需求，要利用React實作Resposive Menu Bar。

## Install react-icons
Import populat icons in popular React projects.\
https://react-icons.github.io/react-icons/
```
npm install react-icons
```

## 範例程式 Header.jsx
```jsx
import { React, useState } from 'react';
import Logo from './assets/logo.svg'
import { AiOutlineMenu, AiOutlineClose, } from 'react-icons/ai';
import styles from './Header.module.css'

const Header = () => {
  // Create a state variable 'nav' and a function 'setNav' to toggle the navigation menu
  const [nav, setNav] = useState(false);
  return (
    <>
      <header>
        <img src={Logo} alt='Logo' />
        <nav>
          <ul className={nav ? [styles.menu, styles.active].join(' ') : [styles.menu]} >
            <li>
              <a href='/#'>Home</a>
            </li>
            <li>
              <a href='/#'>About us</a>
            </li>
            <li>
              < a href='/#'>Contact us</a>
            </li>
          </ul>
        </nav>
        {/* Create a button to toggle the mobile menu */}
        <div onClick={() => setNav(!nav)} className={styles.mobile_btn}>
          {/* Display the close or menu icon based on the 'nav' state */}
          {nav ? <AiOutlineClose size={30} color='white' /> : <AiOutlineMenu size={30} color='white' />}
        </div>
      </header>
    </>
  );
}

export default Header;
```
<!--more-->
## 範例程式 Header.module.css
```css
.header {
  width: 100%;
  height: 70px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;

  /* Set the header's position to fixed to keep it at the top */
  position: fixed;
  top: 0; /* Stick to the top of the viewport */  
  background-color: #0F1F2F; /* Add a background color for the header */  
  z-index: 10; /* Ensure the header is above other content */  
}

.img {
  width: 110px;
  height: 50px;
}

.menu {
  display: flex;
  align-items: center;
}

.menu li {
  padding: 1rem;
}

.header a {
  color: #ffffff;
  font-size: 1.2rem;
}

/* Initially hide the mobile button for small screens */
.mobile_btn {
  display: none;
}

/* Media query for screens with a maximum width of 720px (mobile devices) */
@media screen and (max-width: 720px) {

  /* Display the mobile button when the screen size is smaller */
  .mobile_btn {
    display: block;
    position: absolute;
    right: 1rem;
    cursor: pointer;
    z-index: 10;
  }

  .menu {
    display: flex;
    position: fixed;
    flex-direction: column;
    justify-content: center;
    left: -100%; /* Initially hidden off-screen to the left */
    top: 0;
    bottom: 0;
    right: 0;
    width: 100%;
    height: 100vh;
    background-color: #0F1F2F;
    z-index: 10; /* Ensure the menu is above other content */
    transition: left 1s; /* Add a smooth transition effect */
  }

  /* When the 'active' class is applied, slide the menu in from the left */
  .active {
    left: 0;
  }

  /* Adjust link font size for mobile screens */
  .header a {
    font-size: 2rem;
  }
}
```
## 成果
網頁版
![](/wp-content/uploads/2023/10/Menu-Bar-1.jpg)\
手機版\
![](/wp-content/uploads/2023/10/Menu-Bar-2.jpg)\
點開Menu\
![](/wp-content/uploads/2023/10/Menu-Bar-3.jpg)

## 結語
實作Menu Bar，下次會介紹如何做語言切換。
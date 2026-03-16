---
title: "Service Worker and PWA Updates"
url: /refguide/mobile/introduction-to-mobile-technologies/service-worker
weight: 20
---

## Introduction

A Service Worker is a powerful type of web worker, essentially a JavaScript file that runs in the background, separate from your main web page. It can control pages within its [scope](#scope), and acts as a proxy, sitting between your PWA and the network.

Its primary role is to intercept network requests made by your PWA, allowing it to decide whether to fetch resources from the network or retrieve them from the cache. This interception capability is crucial for providing robust offline experiences, enabling your PWA to function even when the user has no network connectivity. 

## Service worker Scope {#scope}

A service worker’s scope is determined by the location of its JavaScript file on the web server.

If a service worker runs on a page located at /subdir/index.html, and is located at /subdir/sw.js, the service worker's scope is /subdir/

Scope limits which pages are controlled by a service worker, not which requests it can intercept. Once a service worker controls a page, it can intercept any network request that page makes, including requests to cross-origin resources.

## The Service Worker Life Cycle

Understanding the Service Worker life cycle is key to understand how updates to your Mendix PWA are handled. A Service Worker goes through several distinct phases:

1. Registration: 
    This is the initial step. The browser downloads the Service Worker file. If the code contains syntax errors, registration fails, and the Service Worker is discarded.
2. Installation
    During this phase, the Service Worker typically caches static assets your PWA needs to function offline. If all assets are successfully precached, the installation succeeds. If installation fails the service worker is discarded.
    Once installed:
    - It becomes activated immediately if no other Service Worker is currently controlling the page.
    - If there is already an active Service Worker, the new one will be installed but enters a waiting state.
    The "waiting" state, ensures that updates to your application are delivered smoothly and without interrupting your users' current interactions.
3. Activation: 
    Once installed, the Service Worker enters the activating state and then becomes activated. An activated Service Worker takes control of pages within its scope, that means it is ready to intercept requests.
4. Redundant:
    A Service Worker can become redundant if a new version replaces it, or if it fails to install.

## Service Worker Update

When you deploy a new version of your Mendix PWA, a new Service Worker file is generated with updated caching strategies and assets list.
The browser detects this update and initiates a new life cycle for the updated Service Worker.

1. New Service worker installation: The new Service Worker starts to install and cache assets.
2. Waiting state: Once the new Service Worker was successfully installed, it enters a waiting state. The old Service Worker remains active and in control of your PWA.
3. Activation of the new service worker: The new Service Worker will only activate and take control when all instances of your PWA (all open tabs or windows) that are controlled by the old Service Worker are closed.  

## Detecting and Handling Updates in Your PWA

The browser's ServiceWorkerRegistration interface provides properties and methods to monitor its state and detect updates. Starting from Mendix 11.9.0, a client API is available to skip the waiting phase and immediately activates the new service worker version. 
You can implement a custom update mechanism to provide users with a clear notifications and an option to update to the latest version of your app without requiring them to close all tabs.

Implementation Steps:

1. Listening for Service Worker updates
Create a JavaScript Action to Listen for Service Worker Updates. This action should run when your application starts up to monitor for available updates.

```javascript
export async function JS_ListenForPWAUpdates() {
    if (!('serviceWorker' in navigator)) {
        console.warn('Service Workers are not supported in this browser.');
        return;
    }

    try {
        const registration = await navigator.serviceWorker.getRegistration();

        if (registration) {

            if (registration.waiting) {
                console.log('An update is already available and waiting. Notify the user.');
                // Show a confirmation dialog or call your custom update flow
                // to notify the user that an update is available
            }

            // Detect when a new Service Worker update is found
            registration.addEventListener('updatefound', () => {
                const newWorker = registration.installing;

                if (newWorker) {
                    // Listen for state changes in the new Service Worker
                    newWorker.addEventListener('statechange', () => {
                        // When the new Service Worker becomes 'installed', it means precaching is complete and it is ready to take control.
                        // It enters the waiting state because an existing controller (old SW) is active.
                        if (newWorker.state === 'installed' && navigator.serviceWorker.controller) {
                            console.log('A new update is available. Notify the user.');
                            // Show a confirmation dialog or implement your custom update UI 
                            // to notify the user that an update is available                        
                        }
                    });
                }
            });
        }
    } catch (error) {
        console.error('Error setting up Service Worker update listener:', error);
    }
}
```

2. Notifying users
Implement a custom flow that prompts users to confirm updates when a new version becomes available. This avoids interrupting users during critical operations.

3. Activating the new Service Worker
When the user confirms the update, use the Client API `skipWaiting()` to activate the new Service Worker.

```javascript
import { skipWaiting } from "mx-api/pwa";

/**
 * @returns {Promise<boolean>} A promise that resolves to true if the update was successfully activated and page reloaded, false otherwise.
 */
export async function JS_ActivatePWAUpdate() {
    const activated = await skipWaiting();

    if (activated) {
        console.log("New service worker activated and controlling the page.");
                
        return true;
    } else {
        console.warn("No waiting Service Worker found or activation failed via Mendix API.");
        return false;
    }
}
```

4. Reload the page to load the new app version with the newly activated Service Worker, providing the user with the latest features and fixes.

// ==UserScript==
// @name         Posts.tf Auto Bumper 
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  Automatically clicks all bump buttons.
// @author       starbot
// @match        https://posts.tf/users/*
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    // Function to click bump buttons with a 2-second delay between each
    function clickBumpButtons() {
        // Select all bump buttons (up arrow buttons specifically by SVG path)
        const bumpButtons = document.querySelectorAll('button.post-action-button.border-new-theme.has-tooltip svg[data-icon="arrow-up"]');

        console.log(`Found ${bumpButtons.length} bump button(s).`);

        // If there are any bump buttons, we proceed
        if (bumpButtons.length > 0) {
            let index = 0;

            // Function to click a button and log the step
            function clickNextButton() {
                if (index < bumpButtons.length && index < 5) {  // Limit to 5 buttons
                    const bumpButton = bumpButtons[index].parentElement;  // Get the button from the SVG

                    // Ensure the button hasn't been clicked already
                    if (!bumpButton.classList.contains('bumped')) {
                        bumpButton.click();  // Click the bump button
                        bumpButton.classList.add('bumped');  // Mark the button as clicked
                        console.log(`Bumped post ${index + 1}`);  // Log the action
                    }

                    index++;

                    // Wait for 2 seconds before clicking the next button
                    setTimeout(clickNextButton, 2000);
                }
            }

            // Start clicking the bump buttons
            clickNextButton();
        } else {
            console.log("No bump buttons found.");
        }
    }

    // Function to handle the cooldown logic for bumping
    function handleCooldown() {
        // Try to find the message that indicates cooldown time
        const cooldownMessage = document.querySelector('.bump-message-selector'); // Replace with the actual selector for cooldown message

        if (cooldownMessage) {
            const timeText = cooldownMessage.textContent.trim(); // Get the time text
            const minutesLeft = extractTimeFromMessage(timeText);  // Extract the time left in minutes

            console.log(`Cooldown: ${minutesLeft} minutes remaining.`);

            // If the time remaining is less than 5 minutes, wait and bump again
            if (minutesLeft < 5) {
                console.log(`Waiting for ${minutesLeft} minutes to bump again...`);

                // Wait for the cooldown to expire, then bump again
                setTimeout(function() {
                    console.log("Cooldown over, starting to bump again...");
                    clickBumpButtons();  // Start bumping after cooldown
                }, minutesLeft * 60000);  // Convert minutes to milliseconds
            } else {
                console.log("Cooldown too long. Waiting for the next page refresh.");
            }
        } else {
            console.log("No cooldown message found.");
            clickBumpButtons();  // Start bumping if no cooldown message
        }
    }

    // Function to extract the cooldown time from the message (e.g., "Bump will be available in X minutes")
    function extractTimeFromMessage(message) {
        const regex = /(\d+) minute/;
        const match = message.match(regex);
        if (match && match[1]) {
            return parseInt(match[1]);
        }
        return 0;  // Default to 0 if time can't be parsed
    }

    // Wait for the page to fully load, then wait another 15 seconds before starting to bump
    window.addEventListener('load', function() {
        setTimeout(function() {
            console.log("Starting to bump posts...");
            clickBumpButtons();  // Start the bumping process

            // After bumping, check for cooldown
            handleCooldown();
        }, 15000);  // 15-second delay before starting the bumping
    });
})();

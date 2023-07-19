---
title: "Facebook CAPI"
date: 2018-11-18T12:33:46+10:00
weight: 4
---

The Facebook Conversion API (formerly known as the Server-Side API) allows businesses to send user event data directly to Facebook servers from their websites or apps. This data helps track and measure various user actions, such as purchases, sign-ups, and other valuable conversions.

## Objectives

1. To assist businesses in implementing the Facebook Conversion API to accurately track and measure conversions across their websites and apps.
2. To ensure the successful integration of the Conversion API with clients' existing systems and platforms.
3. To provide expertise and guidance in optimizing the use of the Conversion API for improved ad performance and targeting.
4. Save money in the long run by reducing the cost of ad campaigns and improving the ROI.


### Key Concepts of Facebook Conversion API

- **Server-Side Tracking:** The Conversion API enables server-to-server communication, allowing businesses to send conversion events directly to Facebook servers without relying on browser-based tracking.

- **Enhanced Privacy:** By utilizing the Conversion API, businesses can enhance user privacy as sensitive data is transmitted directly from the server without passing through the user's browser.

- **Data Accuracy and Integrity:** The Conversion API ensures accurate and reliable data tracking, reducing discrepancies caused by ad blockers, browser limitations, or JavaScript errors.

- **Custom Event Configuration:** Businesses can configure and send custom conversion events, enabling precise tracking of specific user actions that align with their marketing goals.

- **Advanced Attribution:** The Conversion API supports advanced attribution models, allowing businesses to attribute conversions accurately across multiple touchpoints and campaigns.

## Services Offered

### 1. Conversion API Integration

Our team of experts will guide you through the process of integrating the Facebook Conversion API into your existing website or app infrastructure. We will work closely with your technical team to ensure a seamless and reliable implementation, even migrating the legacy pixels to the new Conversion API.

#### Example Code:

```javascript
// Server-side code example for Conversion API integration
const axios = require('axios');

const sendConversionEvent = async (eventData) => {
  try {
    const response = await axios.post(
      'https://graph.facebook.com/v13.0/{pixel_id}/events',
      eventData
    );
    console.log('Conversion event sent successfully:', response.data);
  } catch (error) {
    console.error('Failed to send conversion event:', error);
  }
};

// Usage example
const eventData = {
  data: [
    {
      event_name: 'Purchase',
      event_time: Math.floor(Date.now() / 1000),
      user_data: {
        email: 'john.doe@example.com',
      },
      custom_data: {
        product_id: 'abc123',
        price: 19.99,
      },
    },
  ],
  access_token: 'your_access_token',
};

sendConversionEvent(eventData);
```

### 2. Event Mapping and Customization

We assist in mapping and configuring custom conversion events to align with your specific business objectives. Our consultants help you identify and implement the most relevant events for accurate tracking and effective ad optimization.

### 3. Data Integrity and Testing

We conduct thorough testing to ensure the accuracy and integrity of the data transmitted through the Conversion API. Our team validates event tracking, troubleshoots any issues, and provides recommendations to optimize data quality.

### 4. Performance Optimization

We analyze your ad campaigns and Conversion API data to identify opportunities for performance improvement. Our consultants provide insights and recommendations to optimize your ad targeting, attribution models, and overall conversion tracking strategy.

----

For any inquiries or questions, please reach out to our team using the [contact](/contact) page.
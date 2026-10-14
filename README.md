# AutoCare — Your Vehicle. Our Care

A mobile-first React + TypeScript vehicle-service booking project for an E-Business Management assignment. Includes car, bike and scooty services, customer flow, demo authentication, bookings, tracking, simulated payments, coupons, rewards, profile, feedback, notifications, and an admin workspace.

## Demo features

- Demo sign-in and sign-up (local demo identity; no password is sent to a server)
- Service catalogue search and vehicle-category filters: Car, Bike, Scooty and all-vehicle services
- Service details, inclusions, ratings and duration
- Vehicle create/delete for cars, bikes and scooters
- Booking date and time selection, one booking per centre/time slot, add-on services, coupon calculation and simulated checkout
- UPI/card/cash payment selection without collecting card data or charging money
- Booking history, cancellation, service timeline, book again, feedback and notifications
- Customer profile, loyalty points, offers, support links, privacy and cancellation policy
- Demo admin workspace for booking status, price/duration/service catalogue editing and basic analytics
- Responsive mobile UI and PWA manifest/service worker

## Run locally

Requirements: Node.js 20+ and npm.

```bash
npm install
npm run dev
```

Open the local URL printed by Vite. To create a production web build:

```bash
npm run build
npm run preview
```

## Demo accounts

- Customer: tap **Continue as Demo User** on the login screen.
- Admin: sign in as a customer, then choose **Admin dashboard** from the left menu and tap **Continue as Demo Admin**.

The demo admin gate is intentionally not production security. Demo records are stored in browser `localStorage`. They remain on that browser/device and can be cleared from browser site data.

## Install as a mobile app (PWA)

Deploy the built `dist/` folder to an HTTPS host (Firebase Hosting, Netlify, Vercel or another static host). Open the site on Android Chrome and choose **Install app** / **Add to Home screen**. HTTPS is required for service-worker/PWA install in production. The included service worker caches the app shell; it does not sync data between devices.

## Create a native Android APK

This workspace does not include the Android SDK/Gradle build toolchain, and package installation could not be completed here. You can build locally on a computer with Android Studio and the Android SDK installed:

```bash
npm install
npm run build
# Capacitor packages are already listed in package.json
npx cap add android
npx cap sync android
npx cap open android
```

In Android Studio select **Build → Build Bundle(s) / APK(s) → Build APK(s)**. The debug APK is typically written under `android/app/build/outputs/apk/debug/`. For a release APK, configure signing credentials securely in Android Studio. Do not commit signing keys.

## Optional Firebase setup

The app is fully usable in local demo mode without Firebase. To configure a Firebase project:

1. Create a Firebase project and register a Web app.
2. Enable Email/Password under Authentication.
3. Create a Cloud Firestore database and review `firestore.rules` before deploying.
4. Copy `.env.example` to `.env.local` and fill the `VITE_FIREBASE_*` values.
5. Restart Vite after changing environment variables.

`src/firebase.ts` initializes Firebase only when valid config is present. The current interactive demo screens use localStorage so the assignment can be demonstrated without a Firebase project. Connecting every screen to Firestore/Auth is a separate integration step; do not treat the sample rules as a production security audit. For a real deployment, use Firebase Auth for identity, server-side validation/Cloud Functions for booking conflicts and admin roles, least-privilege Firestore rules, App Check, privacy/consent notices and monitoring.

## Sample business rules

- AutoCare Service Centre, Coimbatore; service hours 8 AM–8 PM; sample rating 4.7.
- `AUTO10`: 10% discount.
- `SERVICE300`: ₹300 off when service subtotal is ₹2,000 or more.
- Available booking hours: 8 AM through 7 PM start slots. A slot already booked for the selected date is disabled. Cancelled bookings release the slot.
- Sample payments are simulations only. No real card number, CVV, UPI PIN or banking credential is requested or stored.

## Data model for the assignment

Suggested Firestore collections: `users`, `vehicles`, `services`, `service_centres`, `bookings`, `payments`, `notifications`, `reviews`, `coupons`, `offers`, `service_history`, `admins`. Use Firebase Auth UIDs as ownership identifiers. Payment records should contain only safe transaction references/status, never card credentials. The demo uses localStorage equivalents for customer-facing CRUD and flows.

## E-business mapping

| Requirement | Demo implementation |
|---|---|
| B2C model & catalogue | Service catalogue, vehicle segments and service details |
| Search & selection | Search field, category filters and service detail modal |
| Digital transaction | Booking checkout, coupon engine and simulated UPI/card/cash selection |
| CRM | Customer profile, vehicles, booking history, feedback, notifications and rewards |
| Digital marketing | Home banner, offers and coupons |
| Operations | Service-centre selection, time slots, booking status timeline and admin updates |
| Analytics | Booking counts, booking value, completion count and popular services |
| Security/privacy | Local-only demo mode, privacy notice, no real payment credentials, Firebase rules scaffold |
| Scalability | Optional Firebase initialisation and suggested Firestore collections |

## Testing checklist

- [ ] Sign in with demo user and sign out.
- [ ] Create a new vehicle and remove it.
- [ ] Search services and filter Car, Bike and Scooty.
- [ ] Open service details and start a booking.
- [ ] Reject a past date and a slot already used by another active booking.
- [ ] Add extra services and verify subtotal/discount/total.
- [ ] Try `AUTO10` and `SERVICE300` with a qualifying and non-qualifying total.
- [ ] Complete simulated payment and verify booking ID and booking history.
- [ ] Track status, cancel a booking, book again, and submit feedback on a completed booking.
- [ ] Use demo admin to edit/add a service and update booking status.
- [ ] Test layout at 320px, 375px, tablet and desktop widths.

## Important scope note

This is a college-project demo, not a production workshop booking platform. Real-time inventory, live payment gateway, OTP/email password reset, WhatsApp Business integration, secure role-based admin, Firebase CRUD syncing and server-enforced slot locking require backend configuration and integration before public production use.

## Build the APK with GitHub Actions (cloud build)

A workflow is included at `.github/workflows/android-apk.yml`. Upload this project to a GitHub repository, push to the `main` branch (or use **Actions → Build AutoCare Android APK → Run workflow**), and wait for the workflow to finish. Download the `AutoCare-debug-apk` artifact from the workflow run. This is a debug APK for project demonstration, not a Play Store release. Review GitHub repository visibility and do not add secrets or real user data.

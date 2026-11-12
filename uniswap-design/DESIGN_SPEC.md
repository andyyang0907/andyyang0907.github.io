# UniSwap — Product & UI Design Specification

**Tagline:** A smarter way to exchange on campus.

## 1. Product definition
UniSwap is a campus-first platform for students to circulate useful belongings through three modes: second-hand sale, free giveaway, and item exchange. It is not a conventional e-commerce clone. The first release prioritizes safe, lightweight, in-person handoff on campus.

### Core principles
- Campus context first: campus locations, convenient meeting points, and agreed handoff times.
- Three equally valid circulation modes: Sell, Give away, Exchange.
- Trust through clear profiles, listing history, and transaction states; do not imply formal identity verification in v1.
- Keep the first release local and lightweight. No real payment processing, complex identity verification, cloud chat infrastructure, or logistics integration in v1.

## 2. Visual identity
### Art direction
Premium minimalism inspired by the restraint and polish of Apple ecosystem apps: warm off-white canvas, generous whitespace, editorial typography, large product photography, quiet separators, restrained corner radii, and subtle motion. Avoid generic marketplace templates, bright tech-blue, loud gradients, heavy shadows, and decorative clutter.

### Color tokens
| Token | Hex | Usage |
|---|---|---|
| Forest | #174D3A | Primary actions, active states, brand |
| Sage | #75A88A | Secondary accents, supporting states |
| Canvas | #F8F8F5 | Main app background |
| Surface | #FFFFFF | Cards and sheets |
| Ink | #202522 | Primary text |
| Muted | #858B86 | Secondary text |
| Divider | #E8EBE6 | Hairlines and outlines |
| Warning | #C98542 | Caution / pending states |
| Danger | #C65D55 | Destructive actions |

Use a restrained type scale: large editorial page title, clear section headings, compact metadata. Prefer the platform system font (SF Pro on iOS) with consistent weights. Use sentence case, not all-caps labels.

### Logo
The mark is two opposing flowing arrows forming a continuous exchange loop. It must feel calm, balanced, and recognizable at small sizes. Do not use a shopping cart, price tag, or currency symbol. Use the supplied SVG assets as the source of truth.

## 3. Information architecture
Persistent bottom navigation has five destinations:
1. Home
2. Discover
3. Publish (visually emphasized central action)
4. Messages
5. Profile

Each tab must preserve its own navigation stack and scroll position when switching tabs.

### Home
- Header: greeting / campus context and a compact search entry.
- Category shortcuts in a horizontally scrollable row.
- Editorial feature area for curated campus finds.
- Recommended listing feed using large image-led cards.
- Listing cards show title, price or mode, condition, and lightweight campus/location metadata.
- Tap a card to open Product Detail.

### Discover
- Search field, recent searches, category chips, and filter entry.
- Results list/grid adapts to screen width.
- Filters: mode (Sell / Give away / Exchange), category, condition, price range (Sell only), and campus area.
- Empty state should offer a clear reset action.

### Publish
A guided, low-friction flow:
1. Choose Sell / Give away / Exchange.
2. Add photos (multiple images; reorder and remove).
3. Enter title, description, category, condition.
4. Enter price only for Sell; exchange preference only for Exchange.
5. Choose a campus handoff area and availability note.
6. Preview listing.
7. Publish confirmation.
Use inline validation and preserve entered data when navigating backward.

### Messages
- Conversation list with avatar, name, last message, time, and unread state.
- Chat detail with message bubbles, listing context card, and transaction-intent actions.
- V1 may use local/mock persistence; clearly separate the UI from any future remote messaging service.

### Profile
- Profile header with avatar, display name, campus / department / year fields (user-entered).
- Sections: My Listings, Favorites, My Transactions, Settings.
- Listing and transaction states must be explicit and understandable.

### Supporting pages
- Product Detail
- Full-screen image viewer
- Seller profile
- Transaction intent (Buy / Request giveaway / Propose exchange)
- Handoff appointment confirmation
- Transaction detail
- Search and filter sheet
- Listing preview and publish success

## 4. Key screen behavior
### Product Detail
- Large image gallery near the top.
- Title and price/mode clearly visible.
- Condition, category, description, seller identity, and campus handoff information.
- Primary CTA changes by listing mode: Make an offer / Request item / Propose exchange.
- Favorite control has immediate animated feedback.
- Seller row opens Seller Profile.

### Transaction intent and handoff
- Confirm the intent type before moving forward.
- Appointment selection includes campus location and time agreed by both parties.
- Show a clear pending / confirmed / completed state.
- Do not represent an appointment as a completed transaction.
- No real payment flow in v1.

## 5. Motion and interaction specification
Motion should communicate continuity, hierarchy, and feedback—not decoration. Use Flutter-native animation APIs. Prefer 60 fps, short distances, and interruptible transitions.

### Shared-element and route transitions
1. Home listing card → Product Detail: Hero image expands into gallery; detail text fades/slides in with a short stagger. Reverse Hero on back.
2. Discover listing → Product Detail: same shared image transition and reverse path.
3. Product Detail image → Full-screen viewer: Hero zoom, background fades to near-black; swipe between photos.
4. Product Detail seller avatar → Seller Profile avatar: Hero transition.
5. Product Detail → Transaction Intent: modal bottom sheet rises with a gentle spring; background dims.
6. Transaction Intent → Appointment: selected listing and intent card move into the next page; use a shared-element transition where practical.
7. Publish preview → Product Detail-like preview: selected cover image uses Hero; text sections enter sequentially.
8. Search entry → Search screen: search field expands/morphs into the destination search bar while preserving typed text.
9. Messages list → Chat Detail: horizontal route transition; conversation header remains visually continuous.
10. Profile → My Listings / Favorites / Transactions: short horizontal slide with fade.
11. Filter button → Filter sheet: bottom sheet with spring settle; selected chips animate their state.
12. Listing card → Seller profile / listing subroutes: preserve the originating context and reverse transitions.

### Micro-interactions
13. Favorite toggle: scale pulse + AnimatedSwitcher icon fill; haptic feedback where supported.
14. Bottom navigation: animated active indicator and icon/label emphasis; central Publish action has a subtle press scale.
15. Listing cards: press down scale (very subtle), release spring, image clipping remains stable.
16. Category chips: animated color / border / size change.
17. Search clear: text fades out and clear icon rotates/fades.
18. Photo upload: thumbnail appears with scale + fade; progress indicator for processing.
19. Photo reorder: AnimatedList / implicit movement; dragged tile lifts with shadow and returns with spring.
20. Remove photo: tile shrinks/fades; undo affordance appears briefly.
21. Form validation: invalid field gets a restrained shake or border-color transition; avoid aggressive shaking.
22. Publish button: loading state morphs into success checkmark; success content enters with a stagger.
23. Message send: outgoing bubble scales/fades into place; list scrolls only when appropriate.
24. New message: incoming bubble fades/slides in; unread badge scales in.
25. Appointment selection: selected date/time tile animates border, fill, and checkmark.
26. Appointment confirmed: checkmark draw/scale animation with concise success message.
27. Transaction state change: status pill uses AnimatedSwitcher; timeline node animates to completed.
28. Pull to refresh: custom restrained indicator using brand colors.
29. Empty states: illustration/icon and text use a gentle fade + upward motion.
30. Skeleton loading: low-contrast shimmer; stop shimmer when off-screen.
31. Error feedback: compact shake only for actionable errors; success uses a checkmark, not confetti overload.
32. Page section reveal: short staggered FadeTransition + SlideTransition for first entry only; avoid replaying on every rebuild.

### Recommended Flutter techniques
- Hero with stable, unique tags such as listing-image-${listing.id} and user-avatar-${user.id}; never use a shared constant tag across multiple cards.
- PageRouteBuilder with FadeTransition / SlideTransition for custom routes.
- showModalBottomSheet with isScrollControlled and a custom transition where needed.
- AnimationController + CurvedAnimation + Tween + AnimatedBuilder for bespoke motion.
- AnimatedSwitcher for mode, status, icons, and button states.
- AnimatedList or implicitly animated list widgets for list insert/remove/reorder.
- AnimatedContainer / TweenAnimationBuilder for chips, cards, and selection states.
- Draggable / LongPressDraggable with feedback and spring return for photo reorder.
- Use haptics sparingly and only where the action has meaning.

### Timing guidance
- Tap feedback: 100–180 ms.
- Micro-interactions: 150–250 ms.
- Standard route: 250–350 ms.
- Shared-element Hero: 350–500 ms.
- Staggered entrance: 350–650 ms total.
Use easeOutCubic / easeInOutCubic for standard motion and a restrained spring for draggable or sheet interactions. Respect reduced-motion accessibility settings.

## 6. Functional states
Every interactive control must have a real state change. Define loading, empty, error, success, disabled, and selected states where relevant. Do not leave decorative controls that appear tappable but do nothing. Preserve form drafts and tab navigation state.

## 7. V1 scope and exclusions
### In scope
- Local listing data and local persistence.
- Sell / giveaway / exchange listing types.
- Search and local filters.
- Favorites.
- User-entered profile.
- Local/mock conversation and transaction state flows.
- Campus handoff location and appointment selection.
- Responsive layouts for common iPhone sizes and iPad where practical.

### Out of scope for v1
- Real payments / escrow.
- Government ID or student-card verification.
- Production cloud chat / push notification backend.
- Delivery / courier integration.
- Recommendation AI, complex ranking, or social feed.
- Any claim that a user is officially verified unless a real verification system exists.

## 8. Quality bar
- No overflow on small iPhones.
- Smooth scrolling and transitions; avoid expensive blur on large areas.
- Images use caching, correct aspect ratios, and placeholders.
- Accessible contrast and touch targets.
- Route transitions must be reversible and safe under rapid taps.
- Dispose AnimationControllers; avoid duplicate Hero tags.
- Keep domain models and data repositories separate from widgets.
- Use reusable components, but do not over-engineer the first release.

## 9. Design authority
This document and supplied assets are the product/design source of truth. Implementation decisions may adapt technical details but must not change product positioning, navigation, visual identity, page hierarchy, or motion intent without asking first.

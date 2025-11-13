1
1
Plan: Update Fuel Type Values with "Gasolina" Prefix

 Changes Required:

 Current values: '95', '97', 'Diesel'
 New values: 'Gasolina 95', 'Gasolina 97', 'Diesel'

 Implementation Steps:

 1. Update Convex Schema (convex/schema.ts)

 - Update contracts table fuel type union (line 79)
 - Update cars table fuel type union (line 229)

 2. Update Convex Validators

 - convex/cars/mutations.ts - Update create and update validators
 - convex/cars/queries.ts - Update list, get, and listForContractForm validators
 - convex/contracts.ts - Update contract document validator

 3. Update TypeScript Types

 - src/app/admin/dashboard/autos/page.tsx - Update Car type definition (line 61)
 - src/components/admin/cars/car-form-container.tsx - Update type
 - src/features/contract/components/digital-contract-form.tsx - Update type

 4. Update UI Components

 - src/components/admin/cars/car-form.tsx - Update SelectItems (lines 259-279)
 - src/app/admin/dashboard/autos/page.tsx - Update getFuelTypeLabel function
 (lines 135-141)

 5. Create Data Migration Script (convex/migrations/migrateFuelTypes.ts)

 - Create migration mutation to update existing records
 - Map: '95' → 'Gasolina 95', '97' → 'Gasolina 97', 'Diesel' → 'Diesel'
 - Update both cars and contracts tables
 - Include logging for tracking progress

 6. Run Migration

 - Execute migration in local environment first
 - Test thoroughly
 - Execute in production environment

 7. Verify Changes

 - Check Convex TypeScript compilation (npx convex typecheck)
 - Test car creation/editing forms
 - Test contract creation form
 - Verify data display in tables

 Files to Modify (8 files total):

 1. convex/schema.ts
 2. convex/cars/mutations.ts
 3. convex/cars/queries.ts
 4. convex/contracts.ts
 5. src/components/admin/cars/car-form.tsx
 6. src/app/admin/dashboard/autos/page.tsx
 7. src/components/admin/cars/car-form-container.tsx
 8. src/features/contract/components/digital-contract-form.tsx

 Files to Create (1 file):

 1. convex/migrations/migrateFuelTypes.ts (migration script)

 Estimated Time: 30-45 minutes including testing
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

 Would you like to proceed?

 ❯ 1. Yes, and auto-accept edits
   2. Yes, and manually approve edits
   3. No, keep planning

   2
    Ready to code?

 Here is Claude's plan:
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌
 Fix "Ver archivo" button in document read-only view

 Changes to make:

 1. Hide FileUploadField in view mode (document-form.tsx:501-509)
   - Wrap FileUploadField with conditional: {!isReadOnly && <FileUploadField ... />}
 2. Hide delete button in view mode (document-form.tsx:527-535)
   - The trash icon button should not appear in read-only mode
   - Wrap with: {!isReadOnly && <Button ... />}
 3. Improve "Ver archivo" button styling in view mode (document-form.tsx:517-526)
   - Change from simple <a> tag to a proper Button component for better visibility
   - Make it more prominent in view mode
 4. Optional enhancement:
   - Add a download button next to "Ver archivo" for better UX

 Files to modify:

 - /src/features/documents/components/document-form.tsx

 Expected result:

 - In view mode: Only show existing attachment with "Ver archivo" button (no upload/delete options)
 - In edit mode: Keep current functionality (upload, view, delete)
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

 Would you like to proceed?

 ❯ 1. Yes, and auto-accept edits
   2. Yes, and manually approve edits
   3. No, keep planning


  3
   Fix Documents Modal to Show Complete View

     Problem Analysis

     The documents modal (@modal/(.)[id]/page.tsx) currently only shows the DocumentFormContainer, missing three 
     critical sections that appear in the full-page view:
     1. Archivo adjunto - PDF/image preview and download button
     2. Información adicional - Document metadata cards (type, dates, type-specific details)
     3. Acciones rápidas - Quick links to related car and other documents

     Solution

     Following the established pattern from reservations and cars, I'll update the modal route to include all 
     sections that appear in the full-page view. Both routes should show identical content, just with different 
     wrappers (AdminEntityOverlay for modal vs AdminPageLayout for full page).

     Implementation Steps

     1. Update Documents Modal Route

     File: src/app/admin/dashboard/documentos/@modal/(.)[id]/page.tsx

     Changes:
     - Keep the existing AdminEntityOverlay wrapper
     - Add data fetching with useQuery to get full document data (including car, contract)
     - After DocumentFormContainer, add the three missing sections:
       - Archivo adjunto section: Conditional render if document.attachmentUrl exists
           - PDF iframe preview (600px height) OR image preview
         - Download button with proper link
       - Información adicional section: Metadata cards grid
           - Document Type card (type, category, custom label)
         - Dates card (recordedAt, expiresAt, expiration warnings)
         - Fine Status card (conditional, for fine type)
         - Maintenance Details card (conditional, for maintenance type)
         - Insurance Policy card (conditional, for insurance_policy type)
       - Acciones rápidas section: Quick action buttons
           - "Ver vehículo" button linking to car detail page
         - "Otros documentos" button linking to filtered documents list
     - Use the same styling, components, and imports as the full-page view
     - Maintain responsive grid layouts (grid gap-4 sm:grid-cols-2)

     2. Fix "Ver archivo" Button Issue

     The button in the form component that's not working will be fixed by having the proper attachment preview 
     section in the modal, which includes a working download link.

     3. Ensure Size Consistency

     - Change modal size from 'lg' to 'xl' to accommodate the additional sections
     - This matches the reservations pattern which uses 'xl' for view modals

     Expected Result

     After implementation:
     - Modal and full-page views will show identical content
     - Users can preview PDFs/images directly in the modal
     - All metadata cards will be visible in the modal
     - Quick action links will work from the modal
     - The "Ver archivo" button issue will be resolved by the proper attachment section
     - Modal will use proper size ('xl') to fit all content comfortably


     4
     Ready to code?

 Here is Claude's plan:
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌
 Fix Documents View to Show Complete Content in Both Modal and Full Page

 Problem

 Documents have critical content beyond just the form (PDF preview, metadata cards, quick actions) that users need to
 see. Currently:
 - Modal: Shows only the form
 - Full page: Shows form + file preview + metadata + actions

 This makes no sense - users clicking a document should see ALL the content in the modal, not have to navigate to the
 full page.

 Solution

 Both modal and full-page views should show the SAME complete content:
 1. Document form (view mode)
 2. Archivo adjunto section (file preview + download)
 3. Información adicional section (metadata cards)
 4. Acciones rápidas section (quick links)

 Implementation

 1. Update Modal Route

 File: src/app/admin/dashboard/documentos/@modal/(.)[id]/page.tsx

 Add back all the sections I removed:
 - Keep AdminEntityOverlay wrapper with size='xl'
 - Fetch full document data with useQuery(api.documents.queries.getById)
 - Add loading/error states
 - Inside overlay, wrap content in <div className='space-y-6'>:
   - DocumentFormContainer (form fields)
   - Archivo adjunto section (conditional on document.attachmentUrl)
   - Información adicional section (metadata cards grid)
   - Acciones rápidas section (quick action buttons)

 2. Keep Full Page As Is

 File: src/app/admin/dashboard/documentos/[id]/page.tsx
 - Already has all sections
 - No changes needed

 3. Result

 Both views will show identical content:
 - Users can preview PDFs/images in the modal
 - Users can see all metadata in the modal
 - Users can access quick actions from the modal
 - "Ver archivo" link in the form will work properly
 - Modal is large enough (size='xl') to show everything comfortably

 The "Cancelar" button will close the modal via router.back()
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

 Would you like to proceed?

 ❯ 1. Yes, and auto-accept edits
   2. Yes, and manually approve edits
   3. No, keep planning

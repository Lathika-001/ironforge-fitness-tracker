# Supabase Integration Guide for Zenith Fit Lab

This guide will help you set up and use Supabase with your Zenith Fit Lab project.

## Table of Contents
1. [Project Overview](#project-overview)
2. [Setup Instructions](#setup-instructions)
3. [Database Schema](#database-schema)
4. [Available Functions](#available-functions)
5. [Usage Examples](#usage-examples)
6. [Running the SQL Migration](#running-the-sql-migration)

## Project Overview

This project is integrated with Supabase for backend functionality including:
- User authentication
- Profile management
- Meal logging
- Payment tracking
- Progress entries
- Progress photos
- Subscription management

### Project Configuration
- **Project ID**: ozqqcufkypbxypmbzhhh
- **Project URL**: https://ozqqcufkypbxypmbzhhh.supabase.co
- **Provider**: Supabase
- **Client**: @supabase/supabase-js v2.90.1

## Setup Instructions

### 1. Environment Variables

The `.env` file should already be configured with your Supabase credentials:

```env
VITE_SUPABASE_URL=https://ozqqcufkypbxypmbzhhh.supabase.co
VITE_SUPABASE_PUBLISHABLE_KEY=sb_publishable_JBr18KxtXGG2WcqQCRjLkQ_9vSYNmtZ
```

### 2. Install Dependencies

Dependencies are already installed. If needed, run:

```bash
npm install
```

### 3. Run the SQL Migration

See [Running the SQL Migration](#running-the-sql-migration) section below.

### 4. Start the Development Server

```bash
npm run dev
```

## Database Schema

### Tables

#### 1. profiles
User profile information linked to authentication.
- `id` (UUID, Primary Key): References auth.users
- `email` (TEXT): User email
- `full_name` (TEXT): User's full name
- `avatar_url` (TEXT): URL to profile avatar
- `created_at` (TIMESTAMP): Creation timestamp
- `updated_at` (TIMESTAMP): Last update timestamp

#### 2. meal_logs
Records of user meals and nutritional information.
- `id` (UUID, Primary Key): Unique identifier
- `user_id` (UUID): References profiles.id
- `food_name` (TEXT): Name of the food
- `meal_type` (TEXT): Type of meal (breakfast, lunch, dinner, snack)
- `calories` (NUMERIC): Calorie count
- `protein` (NUMERIC): Protein in grams
- `carbs` (NUMERIC): Carbohydrates in grams
- `fats` (NUMERIC): Fats in grams
- `logged_at` (TIMESTAMP): When the meal was logged
- `created_at` (TIMESTAMP): Creation timestamp

#### 3. payments
Payment transaction records.
- `id` (UUID, Primary Key): Unique identifier
- `user_id` (UUID): References profiles.id
- `transaction_id` (TEXT): Unique transaction identifier
- `amount` (NUMERIC): Payment amount
- `plan_name` (TEXT): Name of the plan purchased
- `billing_period` (TEXT): Billing period (monthly, yearly)
- `card_brand` (TEXT): Credit card brand
- `card_last_four` (TEXT): Last 4 digits of card
- `status` (TEXT): Payment status
- `created_at` (TIMESTAMP): Creation timestamp
- `updated_at` (TIMESTAMP): Last update timestamp

#### 4. progress_entries
User fitness progress measurements.
- `id` (UUID, Primary Key): Unique identifier
- `user_id` (UUID): References profiles.id
- `entry_date` (DATE): Date of progress entry
- `weight` (NUMERIC): Weight in specified unit
- `height` (NUMERIC): Height in specified unit
- `bmi` (NUMERIC): Body Mass Index
- `chest` (NUMERIC): Chest measurement
- `waist` (NUMERIC): Waist measurement
- `hips` (NUMERIC): Hips measurement
- `arms` (NUMERIC): Arms measurement
- `thighs` (NUMERIC): Thighs measurement
- `notes` (TEXT): Additional notes
- `created_at` (TIMESTAMP): Creation timestamp
- `updated_at` (TIMESTAMP): Last update timestamp

#### 5. progress_photos
User progress photo records.
- `id` (UUID, Primary Key): Unique identifier
- `user_id` (UUID): References profiles.id
- `photo_url` (TEXT): URL to the photo
- `photo_type` (TEXT): Type of photo (front, side, back)
- `caption` (TEXT): Photo caption/description
- `taken_at` (TIMESTAMP): When photo was taken
- `created_at` (TIMESTAMP): Creation timestamp

#### 6. subscriptions
User subscription information.
- `id` (UUID, Primary Key): Unique identifier
- `user_id` (UUID): References profiles.id
- `payment_id` (UUID): References payments.id
- `plan_name` (TEXT): Name of the subscription plan
- `amount` (NUMERIC): Subscription amount
- `billing_period` (TEXT): Billing period
- `status` (TEXT): Subscription status (active, cancelled, expired)
- `started_at` (TIMESTAMP): Subscription start date
- `expires_at` (TIMESTAMP): Subscription expiration date
- `created_at` (TIMESTAMP): Creation timestamp
- `updated_at` (TIMESTAMP): Last update timestamp

## Available Functions

### Profile Operations
```typescript
import { getProfile, updateProfile } from '@/lib/database';

// Get user profile
const profile = await getProfile(userId);

// Update user profile
const updated = await updateProfile(userId, {
  full_name: 'John Doe',
  avatar_url: 'https://example.com/avatar.jpg'
});
```

### Meal Log Operations
```typescript
import { getMealLogs, createMealLog, updateMealLog, deleteMealLog } from '@/lib/database';

// Get all meal logs for a user
const logs = await getMealLogs(userId);

// Create a new meal log
const newLog = await createMealLog({
  user_id: userId,
  food_name: 'Chicken Salad',
  meal_type: 'lunch',
  calories: 350,
  protein: 30,
  carbs: 20,
  fats: 15
});

// Update a meal log
const updated = await updateMealLog(logId, {
  calories: 400
});

// Delete a meal log
await deleteMealLog(logId);
```

### Payment Operations
```typescript
import { getPayments, createPayment } from '@/lib/database';

// Get all payments for a user
const payments = await getPayments(userId);

// Create a new payment
const payment = await createPayment({
  user_id: userId,
  transaction_id: 'txn_123456',
  amount: 29.99,
  plan_name: 'Premium',
  billing_period: 'monthly',
  card_brand: 'Visa',
  card_last_four: '4242',
  status: 'completed'
});
```

### Progress Entry Operations
```typescript
import { getProgressEntries, createProgressEntry, updateProgressEntry, deleteProgressEntry } from '@/lib/database';

// Get all progress entries
const entries = await getProgressEntries(userId);

// Create a new progress entry
const entry = await createProgressEntry({
  user_id: userId,
  entry_date: '2024-01-15',
  weight: 75.5,
  bmi: 24.5,
  chest: 100,
  waist: 85,
  notes: 'Feeling great!'
});

// Update a progress entry
const updated = await updateProgressEntry(entryId, {
  weight: 74.8
});

// Delete a progress entry
await deleteProgressEntry(entryId);
```

### Progress Photo Operations
```typescript
import { getProgressPhotos, createProgressPhoto, updateProgressPhoto, deleteProgressPhoto } from '@/lib/database';

// Get all progress photos
const photos = await getProgressPhotos(userId);

// Create a new progress photo
const photo = await createProgressPhoto({
  user_id: userId,
  photo_url: 'https://example.com/photo.jpg',
  photo_type: 'front',
  caption: 'Month 1 progress'
});

// Update a progress photo
const updated = await updateProgressPhoto(photoId, {
  caption: 'Month 1 progress - Front view'
});

// Delete a progress photo
await deleteProgressPhoto(photoId);
```

### Subscription Operations
```typescript
import { getSubscriptions, createSubscription, updateSubscription, getActiveSubscription } from '@/lib/database';

// Get all subscriptions
const subscriptions = await getSubscriptions(userId);

// Create a new subscription
const subscription = await createSubscription({
  user_id: userId,
  payment_id: paymentId,
  plan_name: 'Premium',
  amount: 29.99,
  billing_period: 'monthly',
  status: 'active'
});

// Update a subscription
const updated = await updateSubscription(subscriptionId, {
  status: 'cancelled'
});

// Get active subscription
const active = await getActiveSubscription(userId);
```

### Authentication Hook
```typescript
import { useAuth } from '@/hooks/useAuth';

function MyComponent() {
  const { user, session, loading } = useAuth();

  if (loading) return <div>Loading...</div>;

  if (!user) {
    return <div>Please sign in</div>;
  }

  return <div>Welcome, {user.email}</div>;
}
```

## Usage Examples

### Example 1: Using the Supabase Client Directly
```typescript
import { supabase } from '@/integrations/supabase/client';

// Sign in with email
const { data, error } = await supabase.auth.signInWithPassword({
  email: 'user@example.com',
  password: 'password123'
});

// Sign in with OAuth
const { data, error } = await supabase.auth.signInWithOAuth({
  provider: 'google'
});

// Sign out
const { error } = await supabase.auth.signOut();
```

### Example 2: Real-time Subscription
```typescript
import { useEffect } from 'react';
import { supabase } from '@/integrations/supabase/client';

function MealLogsList({ userId }: { userId: string }) {
  useEffect(() => {
    // Subscribe to meal_logs changes
    const subscription = supabase
      .channel('meal_logs_changes')
      .on(
        'postgres_changes',
        {
          event: '*',
          schema: 'public',
          table: 'meal_logs',
          filter: `user_id=eq.${userId}`
        },
        (payload) => {
          console.log('Change received!', payload);
          // Handle the change (update state, etc.)
        }
      )
      .subscribe();

    return () => {
      subscription.unsubscribe();
    };
  }, [userId]);

  return <div>Meal logs will appear here</div>;
}
```

### Example 3: File Upload for Progress Photos
```typescript
import { supabase } from '@/integrations/supabase/client';

async function uploadProgressPhoto(file: File, userId: string, photoType: string) {
  // Upload file to Supabase Storage
  const fileName = `${userId}/${Date.now()}_${file.name}`;
  const { data: uploadData, error: uploadError } = await supabase.storage
    .from('progress-photos')
    .upload(fileName, file);

  if (uploadError) {
    console.error('Upload error:', uploadError);
    return;
  }

  // Get public URL
  const { data: urlData } = supabase.storage
    .from('progress-photos')
    .getPublicUrl(fileName);

  // Create progress photo record
  const { createProgressPhoto } = await import('@/lib/database');
  await createProgressPhoto({
    user_id: userId,
    photo_url: urlData.publicUrl,
    photo_type: photoType,
    caption: 'New progress photo'
  });
}
```

## Running the SQL Migration

### Option 1: Using Supabase Dashboard (Recommended)
1. Go to your Supabase project dashboard: https://supabase.com/dashboard/project/ozqqcufkypbxypmbzhhh
2. Navigate to **SQL Editor** in the left sidebar
3. Click **New Query**
4. Copy the contents of `supabase/migrations/001_create_initial_tables.sql`
5. Paste it into the SQL editor
6. Click **Run** to execute the migration

### Option 2: Using Supabase CLI
If you have the Supabase CLI installed:

```bash
# Link to your project
supabase link --project-ref ozqqcufkypbxypmbzhhh

# Push the migration
supabase db push
```

### Option 3: Direct SQL Execution
You can also execute the SQL directly using any PostgreSQL client connected to your Supabase database.

### What the Migration Does
The migration script:
1. Enables UUID extension
2. Creates all 6 tables (profiles, meal_logs, payments, progress_entries, progress_photos, subscriptions)
3. Creates indexes for better query performance
4. Sets up automatic `updated_at` triggers
5. Enables Row Level Security (RLS)
6. Creates security policies to ensure users can only access their own data
7. Creates a trigger to automatically create a profile when a new user signs up

## Security Notes

- **Row Level Security (RLS)** is enabled on all tables
- Users can only access their own data
- The `user_id` field in all tables references the authenticated user's ID
- All database operations should be done through the provided helper functions to maintain security

## Next Steps

1. **Set up Authentication Providers**: Configure OAuth providers (Google, GitHub, etc.) in your Supabase project settings
2. **Set up Storage**: Create a storage bucket for progress photos if you plan to use file uploads
3. **Configure Email Templates**: Customize email templates for password reset, email verification, etc.
4. **Add Validation**: Add client-side and server-side validation for form inputs
5. **Implement Error Handling**: Add comprehensive error handling throughout your application

## Troubleshooting

### Connection Issues
- Verify your `.env` file has the correct credentials
- Check that your Supabase project is active
- Ensure you're using the correct project URL and keys

### Authentication Issues
- Make sure email authentication is enabled in your Supabase dashboard
- Check that OAuth providers are properly configured
- Verify RLS policies are set up correctly

### Database Issues
- Ensure the SQL migration has been run successfully
- Check that all tables exist in your Supabase dashboard
- Verify indexes are created for better performance

## Support

For more information, visit:
- [Supabase Documentation](https://supabase.com/docs)
- [Supabase Dashboard](https://supabase.com/dashboard)
- [React Integration Guide](https://supabase.com/docs/guides/auth/social-login/auth-react)
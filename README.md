# LUXETHREADS
We believe good clothing should feel like you — not like a costume.
<?php
session_start();

if (!isset($_SESSION['csrf_token'])) {
    $_SESSION['csrf_token'] = bin2hex(random_bytes(32));
}

function h($value): string
{
    return htmlspecialchars((string)$value, ENT_QUOTES, 'UTF-8');
}

function rating_stars($rating): string
{
    $filled = max(0, min(5, (int)round((float)$rating)));
    return str_repeat('★', $filled) . str_repeat('☆', 5 - $filled);
}

function request_value(array $source, string $key): string
{
    $value = $source[$key] ?? '';
    return is_scalar($value) ? (string)$value : '';
}

function photo($id): string
{
    return 'https://images.unsplash.com/photo-' . $id . '?auto=format&fit=crop&w=900&q=85';
}

function redirect_to($path): void
{
    header('Location: ' . $path);
    exit;
}

$products = [
    1 => ['name' => 'Noir Oversized Tee', 'price' => 1299, 'category' => 'T-Shirts', 'tag' => 'BESTSELLER', 'images' => [photo('1521572163474-6864f9cf17ab'), photo('1618354691373-d851c5c3a990')]],
    2 => ['name' => 'Urban Cargo Pants', 'price' => 2499, 'category' => 'Bottoms', 'tag' => 'NEW', 'images' => [photo('1515886657613-9f3515b0c78f'), photo('1542272604-787c3835535d')]],
    3 => ['name' => 'Minimal Linen Shirt', 'price' => 1999, 'category' => 'Shirts', 'tag' => 'TRENDING', 'images' => [photo('1602810318383-e386cc2a3ccf'), photo('1523381210434-271e8be1f52b')]],
    4 => ['name' => 'Everyday Hoodie', 'price' => 2299, 'category' => 'Hoodies', 'tag' => 'HOT', 'images' => [photo('1551488831-00ddcb6c6bd3'), photo('1556821840-3a63f95609a7')]],
    5 => ['name' => 'Classic Denim Jacket', 'price' => 2999, 'category' => 'Jackets', 'tag' => 'LIMITED', 'images' => [photo('1543076447-215ad9ba6923'), photo('1516257984-b1b4d707412e')]],
    6 => ['name' => 'Relaxed Knit Polo', 'price' => 1799, 'category' => 'T-Shirts', 'tag' => 'NEW', 'images' => [photo('1625910513413-5fc45e2a5c10'), photo('1581655353564-df123a1eb820')]],
    7 => ['name' => 'Straight Fit Denim', 'price' => 2699, 'category' => 'Bottoms', 'tag' => 'BESTSELLER', 'images' => [photo('1542272604-787c3835535d'), photo('1541099649105-f69ad21f3246')]],
    8 => ['name' => 'Premium Bomber', 'price' => 3499, 'category' => 'Jackets', 'tag' => 'PREMIUM', 'images' => [photo('1551028719-00167b16eac5'), photo('1551488831-00ddcb6c6bd3')]],
    9 => ['name' => 'Everyday White Tee', 'price' => 999, 'category' => 'T-Shirts', 'tag' => 'ESSENTIAL', 'images' => [photo('1503342217505-b0a15ec3261c'), photo('1521572163474-6864f9cf17ab')]],
    10 => ['name' => 'City Overshirt', 'price' => 2199, 'category' => 'Shirts', 'tag' => 'NEW', 'images' => [photo('1529139574466-a303027c1d8b'), photo('1602810318383-e386cc2a3ccf')]],
    11 => ['name' => 'Weekend Joggers', 'price' => 1899, 'category' => 'Bottoms', 'tag' => 'COMFORT', 'images' => [photo('1503341504253-dff4815485f1'), photo('1542272604-787c3835535d')]],
    12 => ['name' => 'Cloud Fleece Hoodie', 'price' => 2599, 'category' => 'Hoodies', 'tag' => 'COZY', 'images' => [photo('1556821840-3a63f95609a7'), photo('1551488831-00ddcb6c6bd3')]],
    13 => ['name' => 'Utility Field Jacket', 'price' => 3899, 'category' => 'Jackets', 'tag' => 'NEW', 'images' => [photo('1516257984-b1b4d707412e'), photo('1551028719-00167b16eac5')]],
    14 => ['name' => 'Soft Knit Cardigan', 'price' => 2799, 'category' => 'Knitwear', 'tag' => 'LIMITED', 'images' => [photo('1523398002811-999ca8dec234'), photo('1581655353564-df123a1eb820')]],
    15 => ['name' => 'Ribbed Tank Top', 'price' => 899, 'category' => 'T-Shirts', 'tag' => 'ESSENTIAL', 'images' => [photo('1503341455253-d2e30dc48f3e'), photo('1503342217505-b0a15ec3261c')]],
    16 => ['name' => 'Relaxed Oxford Shirt', 'price' => 2399, 'category' => 'Shirts', 'tag' => 'CLASSIC', 'images' => [photo('1512436991641-6745cdb1723f'), photo('1523381210434-271e8be1f52b')]],
    17 => ['name' => 'Everyday Midi Dress', 'price' => 2899, 'category' => 'Dresses', 'tag' => 'NEW', 'images' => [photo('1529139574466-a303027c1d8b'), photo('1503342217505-b0a15ec3261c')]],
    18 => ['name' => 'Pleated Summer Skirt', 'price' => 1699, 'category' => 'Bottoms', 'tag' => 'TRENDING', 'images' => [photo('1515886657613-9f3515b0c78f'), photo('1503341504253-dff4815485f1')]],
    19 => ['name' => 'Canvas Everyday Tote', 'price' => 799, 'category' => 'Accessories', 'tag' => 'EASY CARRY', 'images' => [photo('1490114538077-0a7f8cb49891'), photo('1523398002811-999ca8dec234')]],
    20 => ['name' => 'Soft Wool Scarf', 'price' => 1199, 'category' => 'Accessories', 'tag' => 'SEASONAL', 'images' => [photo('1520975682031-ae8a9c5e0b46'), photo('1512436991641-6745cdb1723f')]],
    21 => ['name' => 'Structured Blazer', 'price' => 4299, 'category' => 'Jackets', 'tag' => 'PREMIUM', 'images' => [photo('1529139574466-a303027c1d8b'), photo('1516257984-b1b4d707412e')]],
    22 => ['name' => 'Textured Camp Collar Shirt', 'price' => 2099, 'category' => 'Shirts', 'tag' => 'JUST IN', 'images' => [photo('1523381210434-271e8be1f52b'), photo('1512436991641-6745cdb1723f')]],
    23 => ['name' => 'Studio Sweatshirt', 'price' => 2199, 'category' => 'Hoodies', 'tag' => 'COMFORT', 'images' => [photo('1551488831-00ddcb6c6bd3'), photo('1556821840-3a63f95609a7')]],
    24 => ['name' => 'Relaxed Chino', 'price' => 2299, 'category' => 'Bottoms', 'tag' => 'EVERYDAY', 'images' => [photo('1503341504253-dff4815485f1'), photo('1542272604-787c3835535d')]],
    25 => ['name' => 'Weekend Graphic Tee', 'price' => 1199, 'category' => 'T-Shirts', 'tag' => 'JUST IN', 'images' => [photo('1521572163474-6864f9cf17ab'), photo('1503342217505-b0a15ec3261c')]],
    26 => ['name' => 'Linen Blend Co-ord', 'price' => 3199, 'category' => 'Sets', 'tag' => 'TRENDING', 'images' => [photo('1523381210434-271e8be1f52b'), photo('1512436991641-6745cdb1723f')]],
    27 => ['name' => 'Tailored Wide-leg Trousers', 'price' => 2599, 'category' => 'Bottoms', 'tag' => 'NEW', 'images' => [photo('1542272604-787c3835535d'), photo('1541099649105-f69ad21f3246')]],
    28 => ['name' => 'Soft Touch Zip Hoodie', 'price' => 2899, 'category' => 'Hoodies', 'tag' => 'BESTSELLER', 'images' => [photo('1556821840-3a63f95609a7'), photo('1551488831-00ddcb6c6bd3')]],
    29 => ['name' => 'Everyday Denim Shirt', 'price' => 2399, 'category' => 'Shirts', 'tag' => 'EASY STYLE', 'images' => [photo('1602810318383-e386cc2a3ccf'), photo('1523381210434-271e8be1f52b')]],
    30 => ['name' => 'Printed Midi Dress', 'price' => 3299, 'category' => 'Dresses', 'tag' => 'NEW', 'images' => [photo('1515886657613-9f3515b0c78f'), photo('1529139574466-a303027c1d8b')]],
    31 => ['name' => 'Quilted Weekend Jacket', 'price' => 3999, 'category' => 'Jackets', 'tag' => 'LIMITED', 'images' => [photo('1551028719-00167b16eac5'), photo('1516257984-b1b4d707412e')]],
    32 => ['name' => 'Everyday Mini Backpack', 'price' => 1499, 'category' => 'Accessories', 'tag' => 'JUST IN', 'images' => [photo('1490114538077-0a7f8cb49891'), photo('1523398002811-999ca8dec234')]],
    33 => ['name' => 'Weekender Canvas Tote', 'price' => 1299, 'category' => 'Bags', 'tag' => 'EVERYDAY', 'images' => [photo('1553062407-98eeb64c6a62'), photo('1584917865442-de89df76afd3')]],
    34 => ['name' => 'City Crossbody Bag', 'price' => 1899, 'category' => 'Bags', 'tag' => 'BESTSELLER', 'images' => [photo('1584917865442-de89df76afd3'), photo('1553062407-98eeb64c6a62')]],
    35 => ['name' => 'Soft Leather Shoulder Bag', 'price' => 2799, 'category' => 'Bags', 'tag' => 'NEW', 'images' => [photo('1548036328-c9fa89d128fa'), photo('1584917865442-de89df76afd3')]],
    36 => ['name' => 'Everyday White Sneakers', 'price' => 2999, 'category' => 'Shoes', 'tag' => 'BESTSELLER', 'images' => [photo('1542291026-7eec264c27ff'), photo('1543163521-1bf539c55dd2')]],
    37 => ['name' => 'Retro Court Sneakers', 'price' => 3299, 'category' => 'Shoes', 'tag' => 'JUST IN', 'images' => [photo('1543163521-1bf539c55dd2'), photo('1542291026-7eec264c27ff')]],
    38 => ['name' => 'Weekend Slides', 'price' => 1199, 'category' => 'Shoes', 'tag' => 'EASY WEAR', 'images' => [photo('1603487742131-4160ec999306'), photo('1543163521-1bf539c55dd2')]],
    39 => ['name' => 'Classic Leather Belt', 'price' => 999, 'category' => 'Belts', 'tag' => 'ESSENTIAL', 'images' => [photo('1624222247344-550fb8f3a5a3'), photo('1618354691373-d851c5c3a990')]],
    40 => ['name' => 'Reversible Everyday Belt', 'price' => 1299, 'category' => 'Belts', 'tag' => 'VERSATILE', 'images' => [photo('1618354691373-d851c5c3a990'), photo('1624222247344-550fb8f3a5a3')]],
    41 => ['name' => 'Ribbed Crew Socks · 3 Pack', 'price' => 499, 'category' => 'Socks', 'tag' => '3-PACK', 'images' => [photo('1586350977771-b3b0abd50c82'), photo('1582966772680-860e372bb558')]],
    42 => ['name' => 'Everyday Ankle Socks · 3 Pack', 'price' => 449, 'category' => 'Socks', 'tag' => 'ESSENTIAL', 'images' => [photo('1582966772680-860e372bb558'), photo('1586350977771-b3b0abd50c82')]],
    43 => ['name' => 'Classic Cotton Baseball Cap', 'price' => 899, 'category' => 'Caps', 'tag' => 'JUST IN', 'images' => [photo('1588850561407-ed78c334e7a5'), photo('1521369909029-2afed882baee')]],
    44 => ['name' => 'Weekend Bucket Hat', 'price' => 999, 'category' => 'Caps', 'tag' => 'SUN READY', 'images' => [photo('1521369909029-2afed882baee'), photo('1588850561407-ed78c334e7a5')]],
    45 => ['name' => 'Minimal Pendant Necklace', 'price' => 1499, 'category' => 'Jewelry', 'tag' => 'NEW', 'images' => [photo('1611652022419-a9419f74343d'), photo('1617038220319-276d3cfab638')]],
    46 => ['name' => 'Everyday Hoop Earrings', 'price' => 1199, 'category' => 'Jewelry', 'tag' => 'BESTSELLER', 'images' => [photo('1617038220319-276d3cfab638'), photo('1611652022419-a9419f74343d')]],
    47 => ['name' => 'Soft Lounge Co-ord Set', 'price' => 2899, 'category' => 'Sets', 'tag' => 'COMFORT', 'images' => [photo('1598033129183-c4f50c736f10'), photo('1591047139829-d91aecb6caea')]],
    48 => ['name' => 'Lightweight Puffer Vest', 'price' => 3199, 'category' => 'Jackets', 'tag' => 'LAYER UP', 'images' => [photo('1591047139829-d91aecb6caea'), photo('1598033129183-c4f50c736f10')]],
    49 => ['name' => 'Relaxed Everyday Shorts', 'price' => 1499, 'category' => 'Bottoms', 'tag' => 'WARM WEATHER', 'images' => [photo('1576566588028-4147f3842f27'), photo('1503341504253-dff4815485f1')]],
    50 => ['name' => 'Soft Cotton Graphic Sweatshirt', 'price' => 2399, 'category' => 'Hoodies', 'tag' => 'NEW', 'images' => [photo('1556821840-3a63f95609a7'), photo('1576566588028-4147f3842f27')]],
];

$uniqueProductPhotos = [
    '1521572163474-6864f9cf17ab', '1515886657613-9f3515b0c78f', '1602810318383-e386cc2a3ccf',
    '1551488831-00ddcb6c6bd3', '1543076447-215ad9ba6923', '1524504388940-b1c1722653e1',
    '1542272604-787c3835535d', '1551028719-00167b16eac5', '1503342217505-b0a15ec3261c',
    '1529139574466-a303027c1d8b', '1503341504253-dff4815485f1', '1556821840-3a63f95609a7',
    '1516257984-b1b4d707412e', '1523398002811-999ca8dec234', '1515377905703-c4788e51af15',
    '1512436991641-6745cdb1723f', '1554568218-0f1715e72254', '1548883354-94bcfe321cbb',
    '1490114538077-0a7f8cb49891', '1547887538-e3a2f32cb1cc', '1564584217132-2271feaeb3c5',
    '1523381210434-271e8be1f52b', '1562157873-818bc0726f68', '1591369822096-ffd140ec948f',
    '1578587018452-892bacefd3f2', '1566206091558-7f218b696731', '1483985988355-763728e1935b',
    '1544441893-675973e31985', '1551232864-3f0890e580d9', '1594633312681-425c7b97ccd1',
    '1485968579580-b6d095142e6e', '1525507119028-ed4c629a60a3', '1553062407-98eeb64c6a62',
    '1584917865442-de89df76afd3', '1548036328-c9fa89d128fa', '1542291026-7eec264c27ff',
    '1543163521-1bf539c55dd2', '1603487742131-4160ec999306', '1575032617751-6ddec2089882',
    '1618354691373-d851c5c3a990', '1586350977771-b3b0abd50c82', '1582966772680-860e372bb558',
    '1566150905458-1bf1fc113f0d', '1521369909029-2afed882baee', '1611652022419-a9419f74343d',
    '1617038220319-276d3cfab638', '1598033129183-c4f50c736f10', '1591047139829-d91aecb6caea',
    '1576566588028-4147f3842f27', '1581655353564-df123a1eb820',
];
foreach (array_keys($products) as $index => $productId) {
    $products[$productId]['images'][0] = photo($uniqueProductPhotos[$index]);
}

$reviewComments = [
    'T-Shirts' => ['The fabric feels lovely and the fit is just right.', 'Easy to style and comfortable enough for all day.', 'The quality is even nicer than I expected.'],
    'Shirts' => ['Looks polished without feeling too formal.', 'Great fabric and a really nice relaxed fit.', 'Wore it all day and it stayed comfortable.'],
    'Bottoms' => ['The fit is comfortable and the finish feels premium.', 'A great everyday pair; sizing was spot on.', 'Easy to dress up or down. Really happy with it.'],
    'Hoodies' => ['Super soft inside and a nice roomy fit.', 'Warm, comfortable, and the colour looks great.', 'This has quickly become my favourite layer.'],
    'Jackets' => ['The details are lovely and it layers really well.', 'Feels well made and looks great in person.', 'A versatile piece that goes with everything.'],
    'Bags' => ['The size is just right and it fits all my daily essentials.', 'Lovely finish and comfortable to carry.', 'Practical, well made, and easy to style.'],
    'Shoes' => ['Comfortable from the first wear and true to size.', 'Great everyday pair; the finish looks even better in person.', 'Easy to walk in and goes with so many outfits.'],
    'Belts' => ['Simple, polished, and the sizing is easy to adjust.', 'The hardware feels sturdy and the finish is great.', 'A useful wardrobe staple with a lovely finish.'],
    'Socks' => ['Soft, comfortable, and they stay in place all day.', 'Great everyday basics; the fabric feels nice.', 'Good fit and a really comfortable feel.'],
    'Caps' => ['A comfortable fit and a great finishing touch.', 'The fabric feels good and it holds its shape nicely.', 'Easy to wear and works with everything.'],
    'Jewelry' => ['A lovely finishing detail and very comfortable to wear.', 'Looks elegant and feels thoughtfully made.', 'Just the right size for everyday styling.'],
    'Accessories' => ['A handy everyday piece with a lovely finish.', 'Thoughtful details and easy to use.', 'A great little addition to my daily rotation.'],
    'Dresses' => ['The fabric drapes beautifully and the fit is comfortable.', 'Lovely for a day out and easy to accessorize.', 'A flattering fit and a really nice finish.'],
    'Knitwear' => ['Soft, cozy, and easy to layer.', 'The knit feels lovely against the skin.', 'A beautiful everyday layer; very happy with it.'],
    'Sets' => ['An easy outfit that feels comfortable all day.', 'The pieces work so well together and separately.', 'Great fit and a lovely soft feel.'],
];
$reviewNames = ['Aarav M.', 'Mira S.', 'Ishaan R.'];
foreach ($products as $productId => $product) {
    $products[$productId]['rating'] = 4.2 + (($productId * 7) % 8) / 10;
    $products[$productId]['review_count'] = 28 + (($productId * 19) % 470);
    $categoryReviews = $reviewComments[$product['category']] ?? $reviewComments['Accessories'];
    $products[$productId]['reviews'] = [];
    for ($reviewIndex = 0; $reviewIndex < 3; $reviewIndex++) {
        $commentIndex = ($productId + $reviewIndex) % count($categoryReviews);
        $products[$productId]['reviews'][] = [
            'name' => $reviewNames[($productId + $reviewIndex) % count($reviewNames)],
            'rating' => 4 + (($productId + $reviewIndex) % 2),
            'comment' => $categoryReviews[$commentIndex],
        ];
    }
}

if (!isset($_SESSION['cart']) || !is_array($_SESSION['cart'])) {
    $_SESSION['cart'] = [];
}
if (!isset($_SESSION['users']) || !is_array($_SESSION['users'])) {
    $_SESSION['users'] = [];
}

$page = request_value($_GET, 'page') ?: 'home';
$allowedPages = ['home', 'login', 'signup', 'account', 'product'];
if (!in_array($page, $allowedPages, true)) {
    $page = 'home';
}
$productId = (int)($_GET['id'] ?? 0);
if ($page === 'product' && !isset($products[$productId])) {
    redirect_to('index.php#shop');
}
$selectedProduct = $page === 'product' ? $products[$productId] : null;
$errorMessage = '';
$currentEmail = $_SESSION['user_email'] ?? '';
$currentUser = $currentEmail !== '' ? ($_SESSION['users'][$currentEmail] ?? null) : null;

if ($page === 'account' && !$currentUser) {
    redirect_to('?page=login');
}

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $token = $_POST['csrf_token'] ?? '';
    $action = request_value($_POST, 'action');
    if (!is_string($token) || !hash_equals($_SESSION['csrf_token'], $token)) {
        $errorMessage = 'Your session expired. Please refresh the page and try again.';
    } else {
        switch ($action) {
            case 'signup':
                $name = trim(request_value($_POST, 'name'));
                $email = strtolower(trim(request_value($_POST, 'email')));
                $password = request_value($_POST, 'password');
                $confirmPassword = request_value($_POST, 'confirm_password');
                if ($name === '' || !filter_var($email, FILTER_VALIDATE_EMAIL) || strlen($password) < 8) {
                    $errorMessage = 'Enter your name, a valid email, and a password with at least 8 characters.';
                } elseif ($password !== $confirmPassword) {
                    $errorMessage = 'Those passwords do not match.';
                } elseif (isset($_SESSION['users'][$email])) {
                    $errorMessage = 'An account already exists for that email. Please log in.';
                } else {
                    $_SESSION['users'][$email] = [
                        'name' => $name,
                        'password' => password_hash($password, PASSWORD_DEFAULT),
                        'orders' => [],
                        'saved' => [],
                    ];
                    session_regenerate_id(true);
                    $_SESSION['user_email'] = $email;
                    redirect_to('?page=account');
                }
                $page = 'signup';
                break;

            case 'login':
                $email = strtolower(trim(request_value($_POST, 'email')));
                $password = request_value($_POST, 'password');
                $account = $_SESSION['users'][$email] ?? null;
                if (!$account || !password_verify($password, $account['password'])) {
                    $errorMessage = 'Email or password is incorrect.';
                } else {
                    session_regenerate_id(true);
                    $_SESSION['user_email'] = $email;
                    redirect_to(request_value($_POST, 'next') === 'checkout' ? 'index.php#checkout' : '?page=account');
                }
                $page = 'login';
                break;

            case 'logout':
                unset($_SESSION['user_email']);
                session_regenerate_id(true);
                redirect_to('index.php');

            case 'add':
                $id = (int)($_POST['product_id'] ?? 0);
                if (isset($products[$id])) {
                    $_SESSION['cart'][$id] = min(99, (int)($_SESSION['cart'][$id] ?? 0) + 1);
                }
                $returnTo = request_value($_POST, 'return_to');
                if ($returnTo === 'product' && isset($products[$id])) {
                    redirect_to('index.php?page=product&id=' . $id);
                }
                redirect_to($returnTo === 'account' ? 'index.php#cart' : '#shop');

            case 'quantity':
                $id = (int)($_POST['product_id'] ?? 0);
                $quantity = (int)($_POST['quantity'] ?? 0);
                if (isset($products[$id])) {
                    if ($quantity < 1) {
                        unset($_SESSION['cart'][$id]);
                    } else {
                        $_SESSION['cart'][$id] = min(99, $quantity);
                    }
                }
                redirect_to('#cart');

            case 'remove':
                unset($_SESSION['cart'][(int)($_POST['product_id'] ?? 0)]);
                redirect_to('#cart');

            case 'clear':
                $_SESSION['cart'] = [];
                redirect_to('#cart');

            case 'order_again':
                if (!$currentUser) {
                    redirect_to('?page=login');
                }
                $orderId = request_value($_POST, 'order_id');
                foreach ($currentUser['orders'] as $order) {
                    if ($order['id'] === $orderId) {
                        foreach ($order['items'] as $item) {
                            $id = (int)$item['id'];
                            if (isset($products[$id])) {
                                $_SESSION['cart'][$id] = min(99, (int)($_SESSION['cart'][$id] ?? 0) + (int)$item['quantity']);
                            }
                        }
                        redirect_to('index.php#cart');
                    }
                }
                $errorMessage = 'That order could not be found.';
                $page = 'account';
                break;

            case 'wishlist':
                if (!$currentUser) {
                    redirect_to('?page=login');
                }
                $id = (int)($_POST['product_id'] ?? 0);
                if (isset($products[$id])) {
                    $saved = $currentUser['saved'] ?? [];
                    if (in_array($id, $saved, true)) {
                        $saved = array_values(array_diff($saved, [$id]));
                    } else {
                        $saved[] = $id;
                    }
                    $_SESSION['users'][$currentEmail]['saved'] = $saved;
                }
                $returnTo = request_value($_POST, 'return_to');
                if ($returnTo === 'product' && isset($products[$id])) {
                    redirect_to('index.php?page=product&id=' . $id);
                }
                redirect_to($returnTo === 'account' ? '?page=account&tab=saved' : '#shop');

            case 'checkout':
                if (!$currentUser) {
                    redirect_to('?page=login&next=checkout');
                }
                $name = trim(request_value($_POST, 'name'));
                $email = strtolower(trim(request_value($_POST, 'email')));
                $address = trim(request_value($_POST, 'address'));
                $city = trim(request_value($_POST, 'city'));
                $pin = trim(request_value($_POST, 'pin'));
                $payment = request_value($_POST, 'payment');
                $items = [];
                $subtotal = 0;
                foreach ($_SESSION['cart'] as $id => $quantity) {
                    if (isset($products[$id]) && (int)$quantity > 0) {
                        $item = $products[$id];
                        $items[] = ['id' => (int)$id, 'name' => $item['name'], 'price' => $item['price'], 'quantity' => (int)$quantity];
                        $subtotal += $item['price'] * (int)$quantity;
                    }
                }
                if (!$items) {
                    $errorMessage = 'Add something to your bag before checking out.';
                    $page = 'home';
                } elseif ($name === '' || !filter_var($email, FILTER_VALIDATE_EMAIL) || $address === '' || $city === '' || !preg_match('/^[0-9]{6}$/', $pin)) {
                    $errorMessage = 'Please complete all delivery details and enter a 6-digit PIN code.';
                    $page = 'home';
                } elseif (!in_array($payment, ['upi', 'cod'], true)) {
                    $errorMessage = 'Choose UPI or cash on delivery to continue.';
                    $page = 'home';
                } else {
                    $shipping = $subtotal > 0 && $subtotal < 2500 ? 99 : 0;
                    $orderId = strtoupper(substr(bin2hex(random_bytes(5)), 0, 10));
                    $_SESSION['users'][$currentEmail]['orders'][] = [
                        'id' => $orderId,
                        'created_at' => date('d M Y, g:i a'),
                        'items' => $items,
                        'subtotal' => $subtotal,
                        'shipping' => $shipping,
                        'total' => $subtotal + $shipping,
                        'payment' => $payment,
                        'status' => $payment === 'upi' ? 'Payment pending (demo)' : 'Confirmed · cash on delivery',
                        'delivery' => ['name' => $name, 'email' => $email, 'address' => $address, 'city' => $city, 'pin' => $pin],
                    ];
                    $_SESSION['cart'] = [];
                    $_SESSION['last_order_id'] = $orderId;
                    redirect_to('?page=account&tab=orders#orders');
                }
                break;
        }
    }
}

$currentEmail = $_SESSION['user_email'] ?? '';
$currentUser = $currentEmail !== '' ? ($_SESSION['users'][$currentEmail] ?? null) : null;
$cartCount = 0;
$cartTotal = 0;
foreach ($_SESSION['cart'] as $id => $quantity) {
    if (isset($products[$id])) {
        $cartCount += (int)$quantity;
        $cartTotal += $products[$id]['price'] * (int)$quantity;
    }
}
$shipping = $cartTotal > 0 && $cartTotal < 2500 ? 99 : 0;
$grandTotal = $cartTotal + $shipping;
$q = trim(request_value($_GET, 'q'));
$categories = ['All', 'T-Shirts', 'Shirts', 'Bottoms', 'Hoodies', 'Jackets', 'Knitwear', 'Dresses', 'Sets', 'Bags', 'Shoes', 'Belts', 'Socks', 'Caps', 'Jewelry', 'Accessories'];
$cat = request_value($_GET, 'cat') ?: 'All';
if (!in_array($cat, $categories, true)) {
    $cat = 'All';
}
$sort = request_value($_GET, 'sort') ?: 'featured';
if (!in_array($sort, ['featured', 'price-low', 'price-high'], true)) {
    $sort = 'featured';
}
$filtered = array_filter($products, function ($product) use ($q, $cat) {
    $matchQuery = $q === '' || stripos($product['name'], $q) !== false || stripos($product['category'], $q) !== false;
    return $matchQuery && ($cat === 'All' || $product['category'] === $cat);
});
if ($sort === 'price-low') {
    uasort($filtered, function ($a, $b) { return $a['price'] <=> $b['price']; });
} elseif ($sort === 'price-high') {
    uasort($filtered, function ($a, $b) { return $b['price'] <=> $a['price']; });
}
$savedProducts = $currentUser ? ($currentUser['saved'] ?? []) : [];
$accountTab = request_value($_GET, 'tab') ?: 'overview';
if (!in_array($accountTab, ['overview', 'orders', 'saved', 'profile'], true)) {
    $accountTab = 'overview';
}
$fallbackImage = 'data:image/svg+xml;base64,' . base64_encode('<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 400 520"><rect width="400" height="520" fill="#eee9e2"/><path d="M140 112l-58 43 34 78 35-18v194h138V215l35 18 34-78-58-43-42 33h-76z" fill="#b9a58e"/><path d="M158 145l42 35 42-35" fill="none" stroke="#fff" stroke-width="8"/></svg>');
?>
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>LUXE THREADS — Modern Clothing</title>
<meta name="description" content="Shop modern everyday clothing at Luxe Threads.">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600;700&family=Playfair+Display:wght@500;600;700&display=swap" rel="stylesheet">
<style>
:root{--ink:#171717;--muted:#6f6f6f;--cream:#f6f3ee;--line:#e8e3dc;--accent:#b38b59;--white:#fff}
*{box-sizing:border-box}html{scroll-behavior:smooth;scroll-padding-top:90px}body{margin:0;background:#fff;color:var(--ink);font-family:'DM Sans',sans-serif}
a{text-decoration:none;color:inherit}button,input{font:inherit}.container{width:min(1180px,92%);margin:auto}
.topbar{background:#171717;color:#fff;text-align:center;padding:10px;font-size:12px;letter-spacing:.14em}
nav{height:76px;display:flex;align-items:center;justify-content:space-between;border-bottom:1px solid var(--line);position:sticky;top:0;background:rgba(255,255,255,.96);backdrop-filter:blur(14px);z-index:10}
.logo{font-family:'Playfair Display';font-size:28px;letter-spacing:.06em}.navlinks{display:flex;gap:26px;font-size:13px}.navlinks a:hover,.account-link:hover{color:var(--accent)}
.icons{display:flex;align-items:center;gap:18px}.cart{position:relative;font-size:18px}.badge{position:absolute;top:-8px;right:-10px;background:#171717;color:#fff;border-radius:20px;font-size:10px;min-width:18px;height:18px;display:grid;place-items:center}
.hero{background:var(--cream);min-height:600px;display:grid;grid-template-columns:1.05fr .95fr;align-items:center;overflow:hidden}
.hero-copy{padding:80px 7vw}.eyebrow{font-size:12px;letter-spacing:.25em;color:var(--accent);font-weight:700}.hero h1{font-family:'Playfair Display';font-size:clamp(52px,7vw,92px);line-height:.92;margin:18px 0}.hero p{color:var(--muted);max-width:480px;line-height:1.8}.btn{display:inline-block;background:#171717;color:#fff;padding:15px 23px;margin-top:18px;border:0;font-size:12px;font-weight:700;letter-spacing:.1em;cursor:pointer;transition:.2s}.btn:hover{transform:translateY(-2px);background:#333}.hero-art{height:600px;background:linear-gradient(145deg,#ddd4c9,#9b8e80);position:relative}.hero-art img{position:absolute;inset:10% 14%;width:72%;height:80%;object-fit:cover;border-radius:2px;filter:grayscale(18%)}
.marquee{border-bottom:1px solid var(--line);border-top:1px solid var(--line);padding:18px 0;overflow:hidden;white-space:nowrap}.marquee span{display:inline-block;margin:0 35px;font-size:11px;letter-spacing:.25em}
.section{padding:78px 0}.section-head{display:flex;justify-content:space-between;align-items:end;margin-bottom:32px}.section h2{font-family:'Playfair Display';font-size:44px;margin:0}.section-sub{color:var(--muted);max-width:450px;line-height:1.6}
.filters{display:flex;gap:9px;flex-wrap:wrap;margin-bottom:26px}.filter{padding:9px 14px;border:1px solid var(--line);font-size:12px}.filter.active,.filter:hover{background:#171717;color:#fff;border-color:#171717}
.grid{display:grid;grid-template-columns:repeat(4,1fr);gap:22px 18px}.card{position:relative;min-width:0}.photo{aspect-ratio:3/4;background:#eee;overflow:hidden;position:relative}.photo img{width:100%;height:100%;object-fit:cover;transition:.5s}.card:hover img{transform:scale(1.035)}.tag{position:absolute;left:12px;top:12px;background:#fff;padding:7px 10px;font-size:10px;letter-spacing:.1em;font-weight:700}
.photo-controls{position:absolute;bottom:12px;left:12px;right:12px;display:flex;gap:6px;justify-content:center}.photo-controls button{border:0;background:rgba(255,255,255,.94);padding:8px 11px;font-size:10px;cursor:pointer}.photo-controls button:hover{background:#171717;color:#fff}.info{padding:12px 2px}.name{font-weight:600}.meta{display:flex;justify-content:space-between;gap:8px;margin-top:6px;color:var(--muted);font-size:13px}.price{color:#171717;font-weight:700}
.banner{background:#171717;color:#fff;padding:68px 20px;text-align:center}.banner h2{font-family:'Playfair Display';font-size:48px;margin:0 0 10px}.banner p{color:#bbb}.banner .btn{background:#fff;color:#171717}
.cartbox,.panel{background:var(--cream);padding:30px;margin-top:18px}.cartrow{display:grid;grid-template-columns:70px 1fr auto auto auto;gap:18px;align-items:center;border-bottom:1px solid #ddd5cc;padding:16px 0}.thumb{width:70px;height:85px;object-fit:cover}.total{text-align:right;font-size:19px;font-weight:700;margin:24px 0}.empty{padding:28px;text-align:center;color:var(--muted)}
.quantity-form{display:flex;align-items:center;gap:6px}.quantity-form button{width:28px;height:28px;border:1px solid #ccc;background:white;cursor:pointer}.remove{color:#765637;text-decoration:underline;font-size:13px}.form-grid{display:grid;gap:14px}.form-grid input,.form-grid select{width:100%;padding:14px;border:1px solid #ddd;background:#fff}.two-col{display:grid;grid-template-columns:1fr 1fr;gap:14px}.payment-choice{display:flex;gap:12px;align-items:flex-start;padding:14px;background:white;border:1px solid #ddd}.payment-choice input{width:auto;padding:0}.notice{padding:13px 15px;background:#fff6e8;border:1px solid #ead3ae;color:#684c2d;margin-bottom:18px}
.auth-wrap{min-height:65vh;display:grid;place-items:center;padding:55px 20px}.auth-card{width:min(480px,100%);background:var(--cream);padding:34px}.auth-card h1,.account-heading h1{font-family:'Playfair Display';font-size:42px;margin:8px 0}.auth-card p{color:var(--muted);line-height:1.6}.auth-card label,.form-grid label{font-size:13px;font-weight:600}.auth-card .form-grid{margin-top:22px}.account-heading{display:flex;justify-content:space-between;align-items:center;gap:20px}.account-cards{display:grid;grid-template-columns:repeat(3,1fr);gap:14px;margin:25px 0}.account-tile{background:var(--cream);padding:20px}.account-tile strong{display:block;font-size:21px;margin-bottom:5px}.order-card{background:var(--cream);padding:22px;margin:16px 0}.order-head{display:flex;justify-content:space-between;gap:15px;flex-wrap:wrap}.order-items{color:var(--muted);font-size:14px;line-height:1.7}.order-card .btn{margin-top:5px}
footer{background:#111;color:#fff;padding:55px 0;margin-top:0}.foot{display:grid;grid-template-columns:2fr 1fr 1fr 1fr;gap:30px}.foot h3{font-family:'Playfair Display';font-size:26px;margin-top:0}.foot p,.foot a{color:#aaa;font-size:13px;line-height:2}.copyright{border-top:1px solid #333;margin-top:35px;padding-top:20px;color:#777;font-size:11px}
.search{display:flex;border:1px solid var(--line);max-width:330px}.search input{border:0;outline:0;padding:12px;width:100%;font:inherit}.search button{border:0;background:#171717;color:#fff;padding:0 15px;cursor:pointer}
@media(max-width:900px){.hero{grid-template-columns:1fr}.hero-art{height:430px}.grid{grid-template-columns:repeat(2,1fr)}.navlinks{display:none}.foot{grid-template-columns:1fr 1fr}.cartrow{grid-template-columns:55px 1fr auto}.cartrow .lineprice{display:none}.account-cards{grid-template-columns:1fr 1fr}}
@media(max-width:560px){.hero-copy{padding:55px 6%}.hero h1{font-size:56px}.section{padding:55px 0}.section-head{display:block}.section h2{font-size:36px;margin-bottom:10px}.grid{gap:12px}.foot{grid-template-columns:1fr}.logo{font-size:21px}.icons{gap:11px}.cartrow{grid-template-columns:50px 1fr auto;gap:10px}.cartrow .quantity-form{grid-column:2}.account-cards{grid-template-columns:1fr}.account-heading{align-items:flex-start;flex-direction:column}.auth-card{padding:25px}.two-col{grid-template-columns:1fr}.banner h2{font-size:38px}}
.topbar{background:#28232a;letter-spacing:.08em}
nav{height:82px;padding:0 max(4%,calc((100% - 1320px)/2));gap:22px}
.logo{font-family:'DM Sans',sans-serif;font-weight:800;letter-spacing:-.06em;color:#e74b75;font-size:27px;white-space:nowrap}
.navlinks{gap:20px;font-weight:700;font-size:12px}
.navlinks a{padding:31px 0;border-bottom:3px solid transparent}
.navlinks a:hover{color:#e74b75;border-color:#e74b75}
.icons{gap:17px;font-size:12px;font-weight:700;white-space:nowrap}
.account-link:hover{color:#e74b75}
.hero{min-height:500px;background:linear-gradient(110deg,#fff1f4,#f8e5d7 55%,#ead1c1);grid-template-columns:1fr 1fr}
.hero-copy{padding:65px 8vw}
.hero h1{font-family:'DM Sans',sans-serif;font-size:clamp(48px,6.2vw,78px);font-weight:800;letter-spacing:-.065em;line-height:.98}
.hero h1 em{color:#e74b75}
.hero-art{height:500px;background:#eedfd6}
.hero-art img{inset:4% 12%;width:76%;height:92%;border-radius:8px;filter:none;box-shadow:0 18px 45px #744c3c2a}
.btn{background:#28232a;border-radius:4px;letter-spacing:.04em}
.btn:hover{background:#e74b75}
.section{padding:62px 0}
.section h2{font-family:'DM Sans',sans-serif;font-weight:800;letter-spacing:-.04em}
.eyebrow{color:#e74b75;letter-spacing:.15em}
.marquee{background:#fffaf6}
.filter{border-radius:20px;padding:9px 16px}
.filter.active,.filter:hover{background:#e74b75;border-color:#e74b75}
.grid{grid-template-columns:repeat(4,minmax(0,1fr));gap:26px 18px}
.card{background:#fff;border-radius:5px;transition:transform .24s ease,box-shadow .24s ease;padding-bottom:4px}
.card:hover{transform:translateY(-7px);box-shadow:0 14px 32px #29202b1a}
.photo{aspect-ratio:3/4;border-radius:5px;background:#f4f1ef}
.photo img{transition:transform .48s cubic-bezier(.2,.8,.2,1)}
.card:hover .photo img{transform:scale(1.075)}
.tag{border-radius:2px;color:#312a32;font-size:9px}
.photo-controls{opacity:0;transform:translateY(7px);transition:.2s}
.card:hover .photo-controls,.card:focus-within .photo-controls{opacity:1;transform:translateY(0)}
.photo-controls button{border-radius:3px;box-shadow:0 2px 8px #0002}
.info{padding:12px 4px 8px}
.name{font-size:14px}
.meta{font-size:12px}
.price{white-space:nowrap}
.product-actions{display:flex;gap:8px}
.product-actions .btn{margin-top:7px;padding:12px 9px}
.catalog-tools{display:flex;align-items:center;justify-content:space-between;color:var(--muted);font-size:13px;margin:0 0 16px}
.catalog-tools form{display:flex;align-items:center;gap:8px}
.catalog-tools select{border:1px solid var(--line);background:#fff;padding:9px 12px;border-radius:4px;color:var(--ink)}
.save-button{margin-top:7px;width:43px;border:1px solid var(--line);border-radius:4px;background:#fff;font-size:19px;cursor:pointer}
.save-button:hover,.save-button.is-saved{color:#e74b75;border-color:#e74b75}
.banner{background:#28232a}
.search{border-radius:4px}
.search button{background:#e74b75}
.account-layout{display:grid;grid-template-columns:230px minmax(0,1fr);gap:30px;margin-top:30px}
.account-nav{display:grid;align-content:start;background:#fff;border:1px solid var(--line);border-radius:6px;overflow:hidden}
.account-nav a{padding:16px 18px;border-bottom:1px solid var(--line);font-size:14px}
.account-nav a:last-child{border-bottom:0}
.account-nav a.active,.account-nav a:hover{color:#e74b75;background:#fff5f7;font-weight:700}
.account-content{min-width:0}
.account-cards{margin-top:0}
.account-tile{background:#fff;border:1px solid var(--line);border-radius:6px}
.order-card,.cartbox,.panel{background:#fff;border:1px solid var(--line);border-radius:6px}
.account-product-grid{display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:16px}
.account-product{border:1px solid var(--line);border-radius:5px;overflow:hidden;padding-bottom:12px}
.account-product img{width:100%;aspect-ratio:3/4;object-fit:cover;display:block}
.account-product strong,.account-product .account-product-actions{display:block;margin:10px 12px}
.account-product .btn{margin:0;padding:10px 12px}
.account-heading{align-items:flex-end}
.auth-card{background:#fff;border:1px solid var(--line);border-radius:8px;box-shadow:0 15px 40px #29202b12}
.auth-card h1{font-family:'DM Sans',sans-serif;font-weight:800}
.auth-card .btn{width:100%}
.payment-choice{border-radius:5px}
footer{background:#28232a}
@media(max-width:900px){nav{padding:0 4%;gap:12px}.navlinks{display:flex;gap:10px}.account-layout{grid-template-columns:1fr}.account-nav{grid-template-columns:repeat(4,1fr)}.account-nav a{text-align:center;border-bottom:0;border-right:1px solid var(--line);padding:13px 6px;font-size:12px}.account-product-grid{grid-template-columns:repeat(2,minmax(0,1fr))}}
@media(max-width:650px){nav{height:auto;min-height:72px;flex-wrap:wrap;padding:12px 4%}.navlinks{order:3;display:flex;width:100%;justify-content:space-between;border-top:1px solid var(--line);padding-top:10px}.navlinks a{padding:7px 0}.logo{font-size:22px}.icons{font-size:11px;gap:10px}.hero{grid-template-columns:1fr}.hero-copy{padding:50px 6%}.hero-art{height:370px}.grid{grid-template-columns:repeat(2,minmax(0,1fr));gap:14px 10px}.photo-controls{opacity:1;transform:none}.photo-controls button{padding:7px 9px}.section{padding:48px 0}.section-head{display:flex;align-items:flex-start;gap:16px;flex-direction:column}.search{max-width:none;width:100%}.account-nav{grid-template-columns:repeat(2,1fr)}.account-nav a{border-bottom:1px solid var(--line)}.account-heading{align-items:flex-start}.account-product-grid{grid-template-columns:repeat(2,minmax(0,1fr))}}
@media(max-width:650px){.catalog-tools{align-items:flex-start;gap:12px;flex-direction:column}}
@media(max-width:650px){nav .container{flex-wrap:wrap}.navlinks{width:100%;margin-left:0}}
@media(prefers-reduced-motion:reduce){*,*::before,*::after{scroll-behavior:auto!important;transition:none!important}}
.photo-link{position:absolute;inset:0;display:block}
.photo-link img{display:block}
.tag,.photo-controls{z-index:1}
.card-rating{display:inline-flex;align-items:center;gap:6px;margin-top:8px;color:#625b57;font-size:12px}
.card-rating .stars,.rating-score .stars{color:#e6a32e;letter-spacing:1px}
.product-detail{display:grid;grid-template-columns:minmax(0,1.05fr) minmax(320px,.95fr);gap:52px;align-items:start}
.detail-gallery{display:grid;grid-template-columns:1fr;gap:12px}
.detail-main-photo{width:100%;aspect-ratio:4/5;object-fit:cover;background:#f4f1ef;border-radius:6px}
.detail-thumbnails{display:flex;gap:10px}
.detail-thumbnails button{width:76px;height:92px;padding:0;border:2px solid transparent;background:#f4f1ef;cursor:pointer}
.detail-thumbnails button[aria-pressed=true]{border-color:#e74b75}
.detail-thumbnails img{width:100%;height:100%;object-fit:cover}
.detail-copy{position:sticky;top:110px}
.detail-copy h1{font-size:clamp(32px,4vw,48px);line-height:1.08;letter-spacing:-.04em;margin:12px 0}
.detail-price{font-size:22px;font-weight:800;margin:18px 0 8px}
.detail-rating{display:flex;align-items:center;gap:10px;color:var(--muted);font-size:14px}
.detail-actions{display:flex;gap:10px;align-items:stretch;margin:24px 0}
.detail-actions .btn{flex:1;text-align:center;margin:0}
.detail-actions .save-button{height:auto;width:52px;margin:0;font-size:22px}
.detail-perks{border-top:1px solid var(--line);border-bottom:1px solid var(--line);padding:16px 0;color:#625b57;font-size:13px;line-height:2}
.rating-panel{display:grid;grid-template-columns:180px 1fr;gap:30px;align-items:center;background:#fff8f5;border:1px solid var(--line);border-radius:8px;padding:24px;margin:22px 0}
.rating-score{text-align:center}.rating-score strong{display:block;font-size:48px;line-height:1.1}.rating-score small{display:block;color:var(--muted);margin-top:5px}
.rating-bars{display:grid;gap:8px}.rating-bar{display:grid;grid-template-columns:32px 1fr 30px;align-items:center;gap:9px;color:#625b57;font-size:12px}
.rating-bar-track{height:8px;background:#eee7df;border-radius:9px;overflow:hidden}.rating-bar-fill{height:100%;background:#e6a32e;border-radius:9px}
.review-list{display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:16px}.review-card{padding:18px;border:1px solid var(--line);border-radius:6px}.review-card strong{font-size:14px}.review-card p{color:#625b57;font-size:13px;line-height:1.65;margin-bottom:0}.review-card .stars{color:#e6a32e;letter-spacing:1px;font-size:13px}
.review-note{font-size:12px;color:var(--muted);margin-top:14px}
.detail-breadcrumb{color:var(--muted);font-size:13px;margin:0 0 22px}
.detail-breadcrumb a:hover{color:#e74b75}
.related-grid{display:grid;grid-template-columns:repeat(4,minmax(0,1fr));gap:18px}
.related-card img{width:100%;aspect-ratio:3/4;object-fit:cover;background:#f4f1ef;border-radius:5px;display:block}
.related-card strong{display:block;font-size:14px;margin-top:10px}.related-card span{display:block;color:var(--muted);font-size:12px;margin-top:4px}
@media(max-width:800px){.product-detail{grid-template-columns:1fr;gap:28px}.detail-copy{position:static}.review-list{grid-template-columns:1fr}.related-grid{grid-template-columns:repeat(2,minmax(0,1fr))}}
@media(max-width:560px){.rating-panel{grid-template-columns:1fr;gap:18px}.rating-score{text-align:left}.detail-thumbnails button{width:62px;height:76px}}
</style>
</head>
<body>
<div class="topbar">STYLE THAT MOVES WITH YOU · FREE SHIPPING ON ORDERS ABOVE ₹2,500</div>
<nav>
<div class="container" style="display:flex;align-items:center;justify-content:space-between;width:100%">
<a class="logo" href="index.php">LUXE THREADS</a>
<div class="navlinks"><a href="index.php#shop">NEW IN</a><a href="index.php?cat=T-Shirts#shop">CLOTHING</a><a href="index.php?cat=Accessories#shop">ACCESSORIES</a><a href="index.php#story">OUR STORY</a></div>
<div class="icons">
<a class="account-link" href="?page=<?= $currentUser ? 'account' : 'login' ?>">♡ &nbsp;<?= $currentUser ? 'MY ACCOUNT' : 'SIGN IN' ?></a>
<a href="index.php#cart" class="cart" aria-label="Shopping bag">BAG 🛍<?php if ($cartCount): ?><span class="badge"><?= $cartCount ?></span><?php endif; ?></a>
</div>
</div>
</nav>

<?php if ($errorMessage !== ''): ?><div class="container notice" role="alert"><?= h($errorMessage) ?></div><?php endif; ?>

<?php if ($page === 'login' || $page === 'signup'): ?>
<main class="auth-wrap">
<section class="auth-card">
<div class="eyebrow">LUXE THREADS ACCOUNT</div>
<h1><?= $page === 'signup' ? 'Create account' : 'Welcome back' ?></h1>
<p><?= $page === 'signup' ? 'Join us to save your orders and make your next visit easier.' : 'Log in to see your order history, saved details and more.' ?></p>
<form method="post" class="form-grid">
<input type="hidden" name="csrf_token" value="<?= h($_SESSION['csrf_token']) ?>">
<input type="hidden" name="action" value="<?= $page ?>">
<?php if ($page === 'signup'): ?><label>Your name<input name="name" autocomplete="name" required></label><?php endif; ?>
<label>Email address<input name="email" type="email" autocomplete="email" required></label>
<label>Password<input name="password" type="password" minlength="8" autocomplete="<?= $page === 'signup' ? 'new-password' : 'current-password' ?>" required></label>
<?php if ($page === 'signup'): ?><label>Confirm password<input name="confirm_password" type="password" minlength="8" autocomplete="new-password" required></label><?php endif; ?>
<?php if ($page === 'login' && ($_GET['next'] ?? '') === 'checkout'): ?><input type="hidden" name="next" value="checkout"><?php endif; ?>
<button class="btn" type="submit"><?= $page === 'signup' ? 'CREATE ACCOUNT' : 'LOG IN' ?></button>
</form>
<p><?= $page === 'signup' ? 'Already have an account? <a href="?page=login"><u>Log in</u></a>' : 'New to Luxe Threads? <a href="?page=signup"><u>Create an account</u></a>' ?></p>
</section>
</main>

<?php elseif ($page === 'account' && $currentUser): ?>
<main class="section container">
<div class="account-heading"><div><div class="eyebrow">YOUR LUXE THREADS</div><h1>Hello, <?= h($currentUser['name']) ?></h1><p class="section-sub"><?= h($currentEmail) ?></p></div>
<form method="post"><input type="hidden" name="csrf_token" value="<?= h($_SESSION['csrf_token']) ?>"><input type="hidden" name="action" value="logout"><button class="btn" type="submit">LOG OUT</button></form></div>
<div class="account-layout">
<aside class="account-nav" aria-label="Account sections">
<a class="<?= $accountTab === 'overview' ? 'active' : '' ?>" href="?page=account&tab=overview">Overview</a>
<a class="<?= $accountTab === 'orders' ? 'active' : '' ?>" href="?page=account&tab=orders">My orders</a>
<a class="<?= $accountTab === 'saved' ? 'active' : '' ?>" href="?page=account&tab=saved">Saved items (<?= count($savedProducts) ?>)</a>
<a class="<?= $accountTab === 'profile' ? 'active' : '' ?>" href="?page=account&tab=profile">My profile</a>
</aside>
<div class="account-content">
<?php if ($accountTab === 'overview'): ?>
<div class="section-head"><div><div class="eyebrow">YOUR DASHBOARD</div><h2>Account overview</h2></div></div>
<div class="account-cards"><a class="account-tile" href="?page=account&tab=orders"><strong><?= count($currentUser['orders']) ?></strong>Orders</a><a class="account-tile" href="?page=account&tab=saved"><strong><?= count($savedProducts) ?></strong>Saved items</a><a class="account-tile" href="index.php#cart"><strong><?= $cartCount ?></strong>Items in your bag</a></div>
<div class="panel"><strong>Keep your style moving</strong><p class="section-sub">Track your purchases, revisit saved pieces, or pick up where you left off.</p><a class="btn" href="index.php#shop">EXPLORE THE COLLECTION</a></div>
<?php elseif ($accountTab === 'orders'): ?>
<div class="section-head"><div><div class="eyebrow">YOUR PURCHASES</div><h2>Order history</h2></div></div>
<?php if (empty($currentUser['orders'])): ?><div class="panel empty">No orders yet. Find a new favorite in the <a href="index.php#shop"><u>latest collection</u></a>.</div>
<?php else: foreach (array_reverse($currentUser['orders']) as $order): ?>
<article class="order-card">
<div class="order-head"><div><strong>Order #<?= h($order['id']) ?></strong><div class="order-items"><?= h($order['created_at']) ?> · <?= h($order['status']) ?></div></div><strong>₹<?= number_format($order['total']) ?></strong></div>
<p class="order-items"><?php foreach ($order['items'] as $itemIndex => $item): ?><?= $itemIndex > 0 ? ' · ' : '' ?><?= h($item['name']) ?> × <?= (int)$item['quantity'] ?><?php endforeach; ?><br>Payment: <?= $order['payment'] === 'upi' ? 'UPI (demo)' : 'Cash on delivery' ?></p>
<form method="post"><input type="hidden" name="csrf_token" value="<?= h($_SESSION['csrf_token']) ?>"><input type="hidden" name="action" value="order_again"><input type="hidden" name="order_id" value="<?= h($order['id']) ?>"><button class="btn" type="submit">ORDER AGAIN</button></form>
</article>
<?php endforeach; endif; ?>
<?php elseif ($accountTab === 'saved'): ?>
<div class="section-head"><div><div class="eyebrow">YOUR WISHLIST</div><h2>Saved items</h2></div></div>
<?php if (!$savedProducts): ?><div class="panel empty">No saved pieces yet. Tap the heart on anything you love while you shop.</div>
<?php else: ?><div class="account-product-grid"><?php foreach ($savedProducts as $savedId): if (!isset($products[$savedId])) { continue; } $product = $products[$savedId]; ?>
<article class="account-product">
<img class="product-image" src="<?= h($product['images'][0]) ?>" data-fallback="<?= h($fallbackImage) ?>" alt="<?= h($product['name']) ?>">
<strong><?= h($product['name']) ?> · ₹<?= number_format($product['price']) ?></strong>
<div class="account-product-actions">
<form method="post" class="product-actions"><input type="hidden" name="csrf_token" value="<?= h($_SESSION['csrf_token']) ?>"><input type="hidden" name="action" value="add"><input type="hidden" name="product_id" value="<?= (int)$savedId ?>"><input type="hidden" name="return_to" value="account"><button class="btn" type="submit">ADD TO BAG</button></form>
<form method="post"><input type="hidden" name="csrf_token" value="<?= h($_SESSION['csrf_token']) ?>"><input type="hidden" name="action" value="wishlist"><input type="hidden" name="product_id" value="<?= (int)$savedId ?>"><input type="hidden" name="return_to" value="account"><button class="remove" type="submit">Remove from saved</button></form>
</div></article>
<?php endforeach; ?></div><?php endif; ?>
<?php else: ?>
<div class="section-head"><div><div class="eyebrow">ACCOUNT DETAILS</div><h2>My profile</h2></div></div>
<div class="panel"><h3><?= h($currentUser['name']) ?></h3><p><?= h($currentEmail) ?></p><p class="section-sub">Your account is ready to keep your orders and saved pieces together.</p></div>
<?php if (!empty($currentUser['orders'])): $userOrders = $currentUser['orders']; $latestOrder = end($userOrders); ?>
<div class="panel"><strong>Most recent delivery details</strong><p><?= h($latestOrder['delivery']['name']) ?><br><?= h($latestOrder['delivery']['address']) ?><br><?= h($latestOrder['delivery']['city']) ?> · <?= h($latestOrder['delivery']['pin']) ?></p></div>
<?php endif; ?>
<?php endif; ?>
</div>
</div>
</main>

<?php elseif ($page === 'product' && $selectedProduct): ?>
<?php
$relatedProducts = array_filter($products, function ($product, $id) use ($selectedProduct, $productId) {
    return $id !== $productId && $product['category'] === $selectedProduct['category'];
}, ARRAY_FILTER_USE_BOTH);
if (count($relatedProducts) < 4) {
    foreach ($products as $id => $product) {
        if ($id !== $productId && !isset($relatedProducts[$id])) {
            $relatedProducts[$id] = $product;
        }
        if (count($relatedProducts) >= 4) {
            break;
        }
    }
}
$relatedProducts = array_slice($relatedProducts, 0, 4, true);
$productIsSaved = in_array($productId, $savedProducts, true);
?>
<main>
<section class="section container">
<p class="detail-breadcrumb"><a href="index.php">Home</a> / <a href="index.php?cat=<?= urlencode($selectedProduct['category']) ?>#shop"><?= h($selectedProduct['category']) ?></a> / <?= h($selectedProduct['name']) ?></p>
<div class="product-detail">
<div class="detail-gallery">
<img id="product-main-image" class="detail-main-photo product-image" src="<?= h($selectedProduct['images'][0]) ?>" data-fallback="<?= h($fallbackImage) ?>" alt="<?= h($selectedProduct['name']) ?> — view 1">
<div class="detail-thumbnails" aria-label="More product photos">
<?php foreach ($selectedProduct['images'] as $view => $image): ?>
<button type="button" data-image="<?= h($image) ?>" data-view="<?= $view + 1 ?>" aria-label="Show product photo <?= $view + 1 ?>" aria-pressed="<?= $view === 0 ? 'true' : 'false' ?>">
<img class="product-image" src="<?= h($image) ?>" data-fallback="<?= h($fallbackImage) ?>" alt="">
</button>
<?php endforeach; ?>
</div>
</div>
<div class="detail-copy">
<div class="eyebrow"><?= h($selectedProduct['tag']) ?> · <?= h($selectedProduct['category']) ?></div>
<h1><?= h($selectedProduct['name']) ?></h1>
<div class="detail-rating"><span class="stars"><?= rating_stars($selectedProduct['rating']) ?></span><strong><?= number_format($selectedProduct['rating'], 1) ?>/5</strong><span><?= number_format($selectedProduct['review_count']) ?> ratings</span></div>
<div class="detail-price">₹<?= number_format($selectedProduct['price']) ?></div>
<p class="section-sub">A considered everyday piece made for easy styling, comfortable wear, and a little more joy in getting dressed.</p>
<div class="detail-actions">
<form method="post" style="flex:1"><input type="hidden" name="csrf_token" value="<?= h($_SESSION['csrf_token']) ?>"><input type="hidden" name="action" value="add"><input type="hidden" name="product_id" value="<?= $productId ?>"><input type="hidden" name="return_to" value="product"><button class="btn" type="submit" style="width:100%">ADD TO BAG · ₹<?= number_format($selectedProduct['price']) ?></button></form>
<?php if ($currentUser): ?>
<form method="post"><input type="hidden" name="csrf_token" value="<?= h($_SESSION['csrf_token']) ?>"><input type="hidden" name="action" value="wishlist"><input type="hidden" name="product_id" value="<?= $productId ?>"><input type="hidden" name="return_to" value="product"><button class="save-button <?= $productIsSaved ? 'is-saved' : '' ?>" type="submit" aria-label="<?= $productIsSaved ? 'Remove from saved items' : 'Save item' ?>"><?= $productIsSaved ? '♥' : '♡' ?></button></form>
<?php else: ?><a class="save-button" href="?page=login" aria-label="Sign in to save item" style="display:grid;place-items:center">♡</a><?php endif; ?>
</div>
<div class="detail-perks">✓ Easy everyday styling<br>✓ Free shipping on orders above ₹2,500<br>✓ Demo store — no real payment is collected</div>
</div>
</div>
</section>

<section class="section container" id="reviews">
<div class="section-head"><div><div class="eyebrow">THE DETAILS</div><h2>Ratings & reviews</h2><p class="section-sub">A quick look at how this piece is rated and what shoppers say.</p></div></div>
<div class="rating-panel">
<div class="rating-score"><strong><?= number_format($selectedProduct['rating'], 1) ?></strong><span class="stars"><?= rating_stars($selectedProduct['rating']) ?></span><small><?= number_format($selectedProduct['review_count']) ?> ratings</small></div>
<div class="rating-bars"><?php foreach ([5 => 74, 4 => 18, 3 => 6, 2 => 1, 1 => 1] as $stars => $share): ?>
<div class="rating-bar"><span><?= $stars ?> star</span><div class="rating-bar-track"><div class="rating-bar-fill" style="width:<?= $share ?>%"></div></div><span><?= $share ?>%</span></div>
<?php endforeach; ?></div>
</div>
<div class="review-list"><?php foreach ($selectedProduct['reviews'] as $review): ?>
<article class="review-card"><div class="stars" aria-label="<?= (int)$review['rating'] ?> out of 5 stars"><?= str_repeat('★', (int)$review['rating']) ?><?= str_repeat('☆', 5 - (int)$review['rating']) ?></div><strong><?= h($review['name']) ?></strong><p><?= h($review['comment']) ?></p></article>
<?php endforeach; ?></div>
<p class="review-note">Ratings and review comments shown here are sample demo content, not verified customer submissions.</p>
</section>

<section class="section container" id="more-like-this">
<div class="section-head"><div><div class="eyebrow">A LITTLE MORE YOU</div><h2>More like this</h2><p class="section-sub">A few more pieces to make your everyday rotation feel like yours.</p></div><a href="index.php?cat=<?= urlencode($selectedProduct['category']) ?>#shop">Shop <?= h($selectedProduct['category']) ?> →</a></div>
<div class="related-grid"><?php foreach ($relatedProducts as $relatedId => $relatedProduct): ?>
<a class="related-card" href="?page=product&id=<?= (int)$relatedId ?>">
<img class="product-image" src="<?= h($relatedProduct['images'][0]) ?>" data-fallback="<?= h($fallbackImage) ?>" alt="<?= h($relatedProduct['name']) ?>" loading="lazy">
<strong><?= h($relatedProduct['name']) ?></strong><span>₹<?= number_format($relatedProduct['price']) ?> · ★ <?= number_format($relatedProduct['rating'], 1) ?></span>
</a>
<?php endforeach; ?></div>
</section>
</main>

<?php else: ?>
<main>
<section class="hero">
<div class="hero-copy"><div class="eyebrow">NEW SEASON · NEW YOU</div><h1>Find your<br><em>everyday</em><br>favourite.</h1><p>Fresh fits, feel-good layers and little details that make the whole look. Meet your next wardrobe repeat.</p><a class="btn" href="#shop">SHOP THE COLLECTION →</a></div>
<div class="hero-art"><img src="<?= h(photo('1485968579580-b6d095142e6e')) ?>" data-fallback="<?= h($fallbackImage) ?>" onerror="this.onerror=null;this.src=this.dataset.fallback" alt="Modern fashion collection"></div>
</section>
<div class="marquee"><span>FRESH ARRIVALS</span><span>EVERYDAY PRICES</span><span>FREE SHIPPING OVER ₹2,500</span><span>NEW FAVOURITES, EVERY WEEK</span></div>

<section class="section container" id="shop">
<div class="section-head"><div><div class="eyebrow">CURATED FOR YOU</div><h2>Latest pieces</h2><p class="section-sub">Explore <?= count($products) ?> wardrobe staples. Open any piece for sample ratings, reviews, and more like it.</p></div>
<form class="search" method="get"><input type="hidden" name="cat" value="<?= h($cat) ?>"><input name="q" value="<?= h($q) ?>" placeholder="Search the collection..." aria-label="Search clothing"><button type="submit" aria-label="Search">⌕</button></form></div>
<div class="filters"><?php foreach ($categories as $category): ?><a class="filter <?= $cat === $category ? 'active' : '' ?>" href="?cat=<?= urlencode($category) ?>&q=<?= urlencode($q) ?>#shop"><?= h($category) ?></a><?php endforeach; ?></div>
<div class="catalog-tools"><span><?= count($filtered) ?> styles to love</span><form method="get"><input type="hidden" name="cat" value="<?= h($cat) ?>"><input type="hidden" name="q" value="<?= h($q) ?>"><label for="sort-products">Sort by </label><select id="sort-products" name="sort" onchange="this.form.submit()"><option value="featured" <?= $sort === 'featured' ? 'selected' : '' ?>>Recommended</option><option value="price-low" <?= $sort === 'price-low' ? 'selected' : '' ?>>Price: low to high</option><option value="price-high" <?= $sort === 'price-high' ? 'selected' : '' ?>>Price: high to low</option></select></form></div>
<div class="grid">
<?php foreach ($filtered as $id => $product): ?>
<article class="card">
<div class="photo">
<a class="photo-link" href="?page=product&id=<?= (int)$id ?>" aria-label="View <?= h($product['name']) ?>"><img class="product-image" data-fallback="<?= h($fallbackImage) ?>" src="<?= h($product['images'][0]) ?>" alt="<?= h($product['name']) ?> — view 1" loading="lazy"></a>
<span class="tag"><?= h($product['tag']) ?></span>
<div class="photo-controls" aria-label="Product photo views"><?php foreach ($product['images'] as $view => $image): ?><button type="button" data-image="<?= h($image) ?>" data-view="<?= $view + 1 ?>">VIEW <?= $view + 1 ?></button><?php endforeach; ?></div>
</div>
<div class="info"><a class="name" href="?page=product&id=<?= (int)$id ?>"><?= h($product['name']) ?></a><div class="meta"><span><?= h($product['category']) ?></span><span class="price">₹<?= number_format($product['price']) ?></span></div><a class="card-rating" href="?page=product&id=<?= (int)$id ?>#reviews" aria-label="<?= number_format($product['rating'], 1) ?> out of 5, <?= (int)$product['review_count'] ?> ratings"><span class="stars"><?= rating_stars($product['rating']) ?></span><span><?= number_format($product['rating'], 1) ?> (<?= number_format($product['review_count']) ?>)</span></a></div>
<div class="product-actions"><form method="post" style="flex:1"><input type="hidden" name="csrf_token" value="<?= h($_SESSION['csrf_token']) ?>"><input type="hidden" name="action" value="add"><input type="hidden" name="product_id" value="<?= (int)$id ?>"><button class="btn" type="submit" style="width:100%">ADD TO BAG +</button></form>
<?php if ($currentUser): ?><form method="post"><input type="hidden" name="csrf_token" value="<?= h($_SESSION['csrf_token']) ?>"><input type="hidden" name="action" value="wishlist"><input type="hidden" name="product_id" value="<?= (int)$id ?>"><button class="save-button <?= in_array($id, $savedProducts, true) ? 'is-saved' : '' ?>" type="submit" aria-label="<?= in_array($id, $savedProducts, true) ? 'Remove from saved items' : 'Save item' ?>"><?= in_array($id, $savedProducts, true) ? '♥' : '♡' ?></button></form><?php else: ?><a class="save-button" href="?page=login" aria-label="Sign in to save item">♡</a><?php endif; ?></div>
</article>
<?php endforeach; ?>
</div>
<?php if (!$filtered): ?><div class="empty">No pieces found. Try another search.</div><?php endif; ?>
</section>

<section class="banner" id="story"><div class="container"><div class="eyebrow" style="color:#c7a06e">LUXE THREADS</div><h2>Less noise. More you.</h2><p>We believe good clothing should feel like you — not like a costume.</p><a class="btn" href="#shop">EXPLORE ALL</a></div></section>

<section class="section container" id="cart">
<div class="section-head"><div><div class="eyebrow">YOUR SELECTION</div><h2>Shopping bag</h2></div></div>
<div class="cartbox">
<?php if (!$cartCount): ?><div class="empty">Your bag is waiting. Add something you love.</div>
<?php else: foreach ($_SESSION['cart'] as $id => $quantity): if (isset($products[$id])): $product = $products[$id]; ?>
<div class="cartrow">
<img class="thumb" src="<?= h($product['images'][0]) ?>" data-fallback="<?= h($fallbackImage) ?>" onerror="this.onerror=null;this.src=this.dataset.fallback" alt="<?= h($product['name']) ?>">
<div><strong><?= h($product['name']) ?></strong><div style="color:#777;font-size:12px"><?= h($product['category']) ?> · ₹<?= number_format($product['price']) ?> each</div></div>
<form method="post" class="quantity-form"><input type="hidden" name="csrf_token" value="<?= h($_SESSION['csrf_token']) ?>"><input type="hidden" name="action" value="quantity"><input type="hidden" name="product_id" value="<?= (int)$id ?>"><button type="submit" name="quantity" value="<?= max(0, (int)$quantity - 1) ?>" aria-label="Decrease quantity">−</button><span><?= (int)$quantity ?></span><button type="submit" name="quantity" value="<?= min(99, (int)$quantity + 1) ?>" aria-label="Increase quantity">+</button></form>
<div class="lineprice">₹<?= number_format($product['price'] * (int)$quantity) ?></div>
<form method="post"><input type="hidden" name="csrf_token" value="<?= h($_SESSION['csrf_token']) ?>"><input type="hidden" name="action" value="remove"><input type="hidden" name="product_id" value="<?= (int)$id ?>"><button class="remove" type="submit" style="background:none;border:0;cursor:pointer">Remove</button></form>
</div>
<?php endif; endforeach; ?>
<div class="total">Subtotal: ₹<?= number_format($cartTotal) ?><br><small style="font-size:12px;color:#777">Shipping: <?= $shipping ? '₹' . number_format($shipping) : 'FREE' ?></small><br>Grand total: ₹<?= number_format($grandTotal) ?></div>
<div style="display:flex;gap:12px;justify-content:flex-end;flex-wrap:wrap"><form method="post"><input type="hidden" name="csrf_token" value="<?= h($_SESSION['csrf_token']) ?>"><input type="hidden" name="action" value="clear"><button class="btn" type="submit">CLEAR BAG</button></form><a class="btn" href="#checkout">CHECKOUT →</a></div>
<?php endif; ?>
</div>
</section>

<section class="section container" id="checkout">
<div class="section-head"><div><div class="eyebrow">SECURE YOUR FAVORITES</div><h2>Ready to order?</h2></div></div>
<?php if (!$cartCount): ?><div class="panel empty">Your bag is empty. Add a piece from the <a href="#shop"><u>collection</u></a> to continue.</div>
<?php else: ?>
<div class="panel" style="max-width:720px">
<?php if (!$currentUser): ?><div class="notice">Please <a href="?page=login&next=checkout"><u>log in</u></a> or <a href="?page=signup"><u>create an account</u></a> to place an order and save it to your history.</div><?php endif; ?>
<form method="post" class="form-grid">
<input type="hidden" name="csrf_token" value="<?= h($_SESSION['csrf_token']) ?>"><input type="hidden" name="action" value="checkout">
<label>Full name<input name="name" autocomplete="name" value="<?= h($currentUser['name'] ?? '') ?>" required></label>
<label>Email address<input name="email" type="email" autocomplete="email" value="<?= h($currentEmail) ?>" required></label>
<label>Delivery address<input name="address" autocomplete="street-address" required></label>
<div class="two-col"><label>City<input name="city" autocomplete="address-level2" required></label><label>PIN code<input name="pin" inputmode="numeric" pattern="[0-9]{6}" maxlength="6" autocomplete="postal-code" placeholder="6 digits" required></label></div>
<strong>Payment method</strong>
<label class="payment-choice"><input type="radio" name="payment" value="upi" required><span><strong>UPI</strong><br><small>Demo order only; a real UPI payment gateway is not connected.</small></span></label>
<label class="payment-choice"><input type="radio" name="payment" value="cod" required><span><strong>Cash on delivery</strong><br><small>Pay when your order arrives.</small></span></label>
<div><strong>Order total: ₹<?= number_format($grandTotal) ?></strong><div style="color:#777;font-size:12px">Includes ₹<?= number_format($shipping) ?> shipping</div></div>
<?php if ($currentUser): ?><button class="btn" type="submit">PLACE DEMO ORDER · ₹<?= number_format($grandTotal) ?></button><?php else: ?><a class="btn" href="?page=login&next=checkout">LOG IN TO CHECK OUT</a><?php endif; ?>
</form>
</div>
<?php endif; ?>
</section>
</main>
<?php endif; ?>

<footer id="contact"><div class="container"><div class="foot">
<div><h3>LUXE THREADS</h3><p>Modern clothing for everyday main characters.</p></div>
<div><h4>SHOP</h4><p><a href="index.php#shop">New arrivals</a><br><a href="index.php#shop">Best sellers</a><br><a href="index.php#shop">Essentials</a></p></div>
<div><h4>HELP</h4><p><a href="#contact">Shipping</a><br><a href="#contact">Returns</a><br><a href="#contact">Size guide</a></p></div>
<div><h4>FOLLOW</h4><p>Instagram<br>Pinterest<br>Threads</p></div>
</div><div class="copyright">© <?= date('Y') ?> Luxe Threads. Built with PHP · Demo ecommerce project.</div></div></footer>
<script>
document.querySelectorAll('.photo-controls button').forEach(function (button) {
    button.addEventListener('click', function () {
        var image = button.closest('.photo').querySelector('.product-image');
        image.onerror = function () {
            image.onerror = null;
            image.src = image.dataset.fallback;
        };
        image.src = button.dataset.image;
        image.alt = image.alt.replace(/view \d+$/, 'view ' + button.dataset.view);
    });
});
document.querySelectorAll('.product-image').forEach(function (image) {
    image.onerror = function () {
        image.onerror = null;
        image.src = image.dataset.fallback;
    };
});
document.querySelectorAll('.detail-thumbnails button').forEach(function (button) {
    button.addEventListener('click', function () {
        var image = document.getElementById('product-main-image');
        image.src = button.dataset.image;
        image.alt = image.alt.replace(/view \d+$/, 'view ' + button.dataset.view);
        document.querySelectorAll('.detail-thumbnails button').forEach(function (thumbnail) {
            thumbnail.setAttribute('aria-pressed', thumbnail === button ? 'true' : 'false');
        });
    });
});
</script>
</body>
</html>
